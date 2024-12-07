---
title: "APIシナリオテストの新ツールrunn"
emoji: "🧪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","openapi","go","test","rest","runn"]
published: true
---

## runn is 何？

３行まとめ
https://twitter.com/katzchum/status/1561490586858770432

今回は API シナリオテストツールの runn をプロジェクトに導入し、一部機能のコントリビュートしながら 3 ヶ月間触ってみておすすめだと感じたことを記事にまとめたいと思います。

## runnとの出会い

4 ヶ月ほど前にスキーマ駆動開発を行っているプロジェクトでいい感じの API のテストをしようと色々 [調査](https://zenn.dev/link/comments/1d69c758bfd222) をしていました。  
その当時は OpenAPI でスキーマ定義して swagger-ui からポチポチ手動テストをしていましたが、API の数も増えるし同じ API でもパターンが結構あり、流石に手動でのテストでは限界があるなーと考えていました。  
パラメータ数も多いので Controller テストで書くにしてもコード量が多く、レビューが辛いと感じていました。  
API を E2E でテストしたい、最終的には CI で自動テストまでもっていきたいと思いました。  

しかし、いざ調べてみると案外ニーズにマッチするツールが見当たりませんでした。いくつか調べた中では

* [Postman](https://www.postman.com/)([Newman](https://github.com/postmanlabs/newman))
* [Karate](https://github.com/karatelabs/karate)
* [Scenarigo](https://github.com/zoncoen/scenarigo)

があり良さそうに思いましたが、微妙にマッチしませんでした。  
API 単体でのテストは上記以外も数多く存在しましたが、API をチェーンして呼び出すシナリオテストをするには難しそうと判断しました。

API をチェーンするというのは例えば

1. 登録 API（コードが発行される）
2. 更新 API（1.で発行されたコードを指定して更新）

という一連の流れがあるケースです。`Chaining Requests` と呼ぶみたいです ^[参考: https://learning.postman.com/labs/postman-flows/getting-started/chaining-requests-with-data/]
Postman では GUI でパラメータ埋め込みをして API を繋げていけそうな雰囲気がありましたが

* GUI で出力したファイル（Collection）が大きすぎてレビューに向かない  
GUI でパラメータを細かく設定していくのもかなり時間がかかり辛かったです。
* シナリオを flows で定義してみましたが、flows を export できず Newman で CI 実行できなさそうでした

他のツールもレスポンスをステートとして引き継いで API を呼び出すということが、スクリプトを書かないといけなさそうな雰囲気があり直感的に使えなさそうでした。^[ツールがカバーする範囲が広く、自分の調べ方が良くなかったかもですが。。]

そうこうしている間にふと、以下の記事が目に留まりました。

https://tech.pepabo.com/2022/06/07/scenario-testing-in-go/

ひと目見てコレだ！という感じでした。  
こちらの記事と [REAMDE](https://github.com/k1LoW/runn) を読めば大凡の使い勝手の理解が出来る可読性の高さを感じました。  

## APIチェーンは簡単

シナリオは Runbook という yaml 形式で記載します。  
Runbook 自体の記述方法は、上記の紹介記事や README をさらっと目を通して貰えれば雰囲気は掴めると思います。  
レスポンスを次のステップで参照できるので、サックとチェーンさせることが出来ます。  


```yaml
steps:
  createUser:
    desc: Create User APIのテスト（IDが発行される）
    req:
      /user:
        post:
          body:
            application/json:
              username: "alice"
              email: "alice@example.com"
    test: steps.createUser.res.status == 200
  updateUser:
    desc: Updated User APIのテスト（aliceをbobに変更する）
    req:
      /user/{{ steps.createUser.res.body.id }}:
        put:
          body:
            application/json:
              username: "bob"
    test: steps.updateUser.res.status == 200
```

`{{ }}` で囲まれた `steps.createUser.res.body.id` の部分が `Create User` したレスポンスデータに含まれる `id` を展開する式です。　　
実行したステップ名（上記例だと `createUser` と `updateUser` ) を指定して

* `ステップ名.res.status`  
レスポンスのステータスコード
* `ステップ名.res.headers['ヘッダー名'][0]`  
レスポンスヘッダ
* `ステップ名.res.body`  
レスポンスのボディ

が参照できます。
例えば以下のような json レスポンスだった場合

```json
{
  "data": {
    "username": "alice",
    "email": "alice@example.com"
  }
}
```

`username` を参照するには `{{ ステップ名.res.body.data.username }}` で展開ができます。
期待値の検証も `test` 構文に同様に書くことができます。

```yml
    test: current.res.status == 200 && current.res.data.email == "alice@example.com"
```

ステップ名が `current` になっていますが、runner の実行ステップと同じステップを参照できる予約語になっています。  
test 構文の場合は `{{  }}` で式を囲まなく直接記載します。

## シナリオを再利用して書ける

vars と include 構文を組み合わせてシナリオを再利用させることができます。  
前述例の API チェーンを再利用させて書くとこんな感じになります。

* createUser.yml
  ```yml
  vars:
    username: "alice"
    email: "alice@example.com"
  steps:
    createUser:
      desc: Create User APIのテスト（IDが発行される）
      req:
        /user:
          post:
            body:
              application/json:
                username: "{{ vars.username }}"
                email: "{{ vars.email }}"
      test: steps.createUser.res.status == 200  
  ```
* updateUser.yml
  ```yml
  vars:
    username: "bob"
    email: "bob@example.com"
  steps:
    createUser:
      desc: Create User APIのテスト（include実行）
      include:
        path: createUser.yml
        vars:
          username: "{{ vars.username }}"
          email: "{{ vars.email }}"
        skipTest: trrue
    updateUser:
      desc: Updated User APIのテスト
      req:
        /user/{{ steps.createUser.res.body.id }}:
          put:
            body:
              application/json:
                username: "sam"
      test: steps.updateUser.res.status == 200  
  ```

include を使用して Create User のテストが再利用されるようになりました。  
vars はシナリオ内で利用できる変数を定義できます。
include 時に vars を上書きできるようになっているので、必要なパラメータを vars にしておくと再利用性が高くなります。  
また `skipTest` を `true` にしておくと、include したいシナリオの test 構文がスキップされるので軽微ですが実行時間の短縮になります。

注意点としてシナリオを DRY に書きすぎると、どこまでも数珠つなぎになってしまいます。  
メンテナンス性や読みやすさを顧慮して適度に共通化する感じが良いです。

## データ駆動テストができる

上記の include と vars を組み合わせしたテストを発展させてデータ駆動テストっぽくシナリオを書いてみます。  
vars の設定で json 読み込みをサポートさせました。  

* user.json
  ```json
  {
    "username": "alice",
    "email": "alice@example.com"
  }
  ```
* createUser1.yml
  ```yml
  vars:
    createUserRequest: "json://path/to/user.json"
  steps:
    createUser:
      desc: Create User APIのテスト（request bodyをjsonファイルで指定）
      req:
        /user:
          post:
            body:
              application/json: "{{ vars.createUserRequest }}"
      test: steps.createUser.res.status == 200  
  ```

リクエスト body の内容をシナリオファイルから追い出すことができました。  
API が複雑でパラメータが多い場合等、json データをそのままリクエストで使えるようになるので大変便利です。  
レスポンスの期待値も json データにしておくのもありです。  

もう少し発展させて、いくつかのデータパターンを繰り返しテストさせたくなります。  
[@k1low](https://twitter.com/k1low) さんと協議して runn に元々あった retry 機能を loop 機能として [リファクタリング＆機能拡張してもらい](https://github.com/k1LoW/runn/pull/97) 以下のようにできました。


* createUserBase.yml
  ```yml
  vars:
    createUserRequest: "json://path/to/user_request.json"
    createUserResponse: "json://path/to/user_response.json"
  steps:
    createUser:
      desc: Create User APIのテスト（リクエストとレスポンスの期待値をjsonデータで受け取る）
      req:
        /user:
          post:
            body:
              application/json: "{{ vars.createUserBody }}"
      test: |
        steps.createUser.res.status == 200
        && compare(steps.createUser.res.body, vars.createUserResponse)
  ```
* createUserPattern.yml
  ```yml
  vars:
    requestJson:
       - "json://path/to/case1_request.json"
       - "json://path/to/case2_request.json"
       - "json://path/to/case3_request.json"
    responseJson:
       - "json://path/to/case1_response.json"
       - "json://path/to/case2_response.json"
       - "json://path/to/case3_response.json"
  steps:
    createUser:
      desc: Create User APIのテスト（jsonファイル数分繰り返しテストする）
      loop:
        count: len(vars.requestJson)
      include:
        path: createUserBase.yml
        vars:
          createUserRequest: "json://{{ vars.requestJson[i] }}"
          createUserResponse: "json://{{ vars.responseJson[i] }}"
  ```


createUserBase.yml ではレスポンスの期待値のチェック様に `compare` という組み込み関数を用意しました。  
もしレスポンス内で毎回値が変わるフィールドがあれば、第 3 引数以降にフィールド名を指定して除外できるようになっています。  

createUserPattern.yml から createUserBase.yml へリクエストと期待値の json を loop 構文を使って渡しています。  
ループカウンターは `i` の部分になります。


## jsonファイル以外にも直接DBからデータを参照して活用できる

他の API ツールにない強力な機能です。  
前述の loop 構文と組み合わせればかなり大量にアクセスができるかと思います w  

```yaml
runners:
  db: my:dbuser:${DB_PASS}@hostname:3306/dbname
steps:
  find_user:
    db:
      query: SELECT * FROM users WHERE email like '%@exmple.com'
  user_info:
    loop:
      count: len(steps.find_user.rows)
    req:
      /users/{{ steps.find_user.rows[i].id }}:
        get:
          body: null
```

リクエストの結果がレスポンスから窺い知れないケースでも、DB へクエリ発行して期待値として検証できるのも地味ですが役に立ちます

## シナリオをシンプルにしデータは柔軟にする

データ駆動テストの例でもありますが、シナリオ自体はごくシンプルにしてデータに柔軟性をもたせることが重要かと考えています。  
試験的な機能ではあるのですが、[Go Template](https://pkg.go.dev/text/template)の機能を利用してデータを生成できる仕組みも組み込んでいます。

やり方は先程の vars 構文で json を読み込む場合と同じで拡張子だけを `.json.template` にするだけです。  
include の vars の上書き時に利用すると、前ステップの結果等を踏まえたデータを生成できます。

* updateUserPattern.yml
  ```yml
    updatedUser:
      desc: Updated User APIのテスト（テンプレートで期待値を生成）
      include:
        path: updateUser.yml
        vars:
          updatedUserRequest: "json://path/to/request.json"
          updatedUserResponse: "json://path/to/response.json.template"
  ```

* response.json.template
  ```json
  {
    "id": {{.steps.createUser.res.body.id}},
    "username": "alice",
    .. snip ..
  }
  ```

vars と変数展開と似ていて紛らわしいのですが、これは Go Template の記法となります。  
テンプレートとして機能し、id の値を埋め込む等できます。    
またテンプレートの構文として if や range が使えるので単純な値の埋め込みだけでなく、よりダイナミックにデータを生成できます。

## 色々な環境でサクッと動かすことができる

go のコマンドとして実装されているので、CI やそれ以外の環境でも簡単に動かすことができます。
シナリオ内で環境変数の参照が可能で、環境毎のエンドポイント等の違いを切り替えることができます。

```yaml
runners:
  req:
    endpoint: ${END_POINT:-https://httpbin.org/}
  db: my:dbuser:${DB_PASS}@${DB_HOST:-localhost}:3306/dbname
```

runners 構文内の設定は上記の様に環境変数を埋め込んでおくと良いです。  
docker-compose のように環境変数の参照時に default 指定ができます。  
普段はローカル用にして、それ以外の環境の場合は環境変数で上書きする感じで使えます。

環境変数を埋め込むのにひと手間かかりますが、デプロイ後の動作検証が runn のコマンド一発で済むようになったりと活用範囲が広がっておすすめです。  

GitHub Actions で簡単に動かせるようにカスタムアクションも用意したので良かったら是非！

https://github.com/k2tzumi/runn-action


## デバック実行とOpenAPIV3対応が親切

シナリオ作成時にはデバック実行させて行うのがベストです。  
デバック実行は runn 実行時のオプションで指定させるのが良いです。

```console
$ runn run path/to/scenario.yml --debug
```

リクエスト及びレスポンスの内容も全て表示されます。  
test 構文でのチェックで字句解析されて結果が以下の様に出力されるので大変わかりやすいです。

```log
Run 'req' on 'testing include'.steps[0]
-----START HTTP REQUEST-----
POST /post?count=0 HTTP/1.1
Host: httpbin.org
Content-Type: application/json
X-Test: default

{"bar":1,"foo":"test"}
-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 523
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Mon, 19 Sep 2022 09:28:25 GMT
Server: gunicorn/19.9.0

{
  "args": {
    "count": "0"
  }, 
  "data": "{\"bar\":1,\"foo\":\"test\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "22", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-63283639-124641461cb7175a1280e0ad", 
    "X-Test": "default"
  }, 
  "json": {
    "bar": 1, 
    "foo": "test"
  }, 
  "origin": "127.0.0.1", 
  "url": "https://httpbin.org/post?count=0"
}

-----END HTTP RESPONSE-----
Run 'test' on 'testing include'.steps[0]
-----START TEST CONDITION-----
current.res.status == 200
&& current.res.body.json == vars.jsonRequestBody

├── current.res.status => 200
├── 200 => 200
├── current.res.body.json => {"bar":1,"foo":"test"}
└── vars.jsonRequestBody => {"bar":1,"foo":"test"}
-----END TEST CONDITION-----
testing include ... ok

1 scenario, 0 skipped, 0 failures
```

また OpenAPI の v3 にも対応していて Spec を以下のように指定しておくと幸せになれます。

```yaml
runners:
  req:
    endpoint: https://api.github.com
    openapi3: path/to/openapi.yaml
```

OpenAPI の仕様書通りのリクエストとレスポンスになってない場合はエラーにしてくれます。  
エラー内容も仕様書の定義を表示してくれるので、シナリオの記述間違いに気付くことができます。  
シナリオ自体の品質を高めるのと一緒にテスト対象の API の品質も同時にチェックできて大変良いです。  
実際に runn に導入していくつか API の不具合を検知することが出来ました。

## 今後の発展にも期待！

今進行中なものとして Golden Test 用に Capture 機能が実装され始めたり、組込み関数が実装しやすく拡張ポイントが提供されていたりもするので今後も期待大です。

## 最後に

素晴らしいツールを世に送り出してくれた [@k1low](https://twitter.com/k1low) さんに感謝です。  
記事を見かけて感じた、可読性の高さと学習の容易性は思惑以上でした。既にプロジェクト内には浸透済みとなり、なくてはならないツールになりました。  
この記事を見て興味を持たれた方は是非試してみて頂きたいと思います。

また私個人（Contributor）としては、ふわっとした提案でも丁寧にディスカッションをして頂き大変ありがたかったです。  
背景や考え方についての学びも多く、何よりも発展的に機能が充実していく様を間近で見られるのが楽しいです。  
今後も微力ながら貢献できていけたらと思います。