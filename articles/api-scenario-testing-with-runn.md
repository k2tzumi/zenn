---
title: "APIシナリオテストの新ツールrunn"
emoji: "🧪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["scenario-testing","automation","api"]
published: false
---

## runnとの出会い

4ヶ月ほど前にスキーマ駆動開発を行っているプロジェクトでいい感じのAPIのテストをしようと色々 [調査](https://zenn.dev/link/comments/1d69c758bfd222) をしていました。  
その当時はOpenAPIでスキーマ定義してswagger-uiからポチポチ手動テストをしていましたが、API数も増えるし同じAPIでもパターンが結構あり、流石に手動でのテストは限界があるなーと考えていました。  
パラメータ数も多いのでControllerテストで書くにしてもコード量が多くなるのと、レビューが辛いと感じていました。  
最終的にはCIで自動テストまでもっていきたいが。。という思いでした。  

しかし、いざ調べてみると案外マッチするツールがありませんでした。いくつか調べた中では

* [Postman](https://www.postman.com/)([Newman](https://github.com/postmanlabs/newman))
* [Karate](https://github.com/karatelabs/karate)
* [Scenarigo](https://github.com/zoncoen/scenarigo)

がありましたが、微妙にマッチしませんでした。  
API単体でのテストはどれも問題なく機能すると感じましたが、APIをチェインして呼び出すシナリオテストをするには難しそうと判断しました。

APIをチェーンするというのは例えば

1. 登録API（コードが発行される）
2. 更新API（1.で発行されたコードを指定して更新）

という一連の流れがあるケースです。  
PostmanではGUIでパラメータ埋め込みをしてAPIを繋げていけそうな雰囲気がありましたが

* GUIで出力したファイル（Collection）が大きすぎてレビューに向かない  
GUIでパラメータを細かく設定していくのもかなり時間がかかり辛かったです。
* シナリオをflowsで定義してみましたが、flowsをexportできずNewmanでCI実行できなさそうでした

他のツールもレスポンスをステートとして引き継いでAPIを呼び出すということが、スクリプトを書かないといけなさそうな雰囲気があり直感的に使えなさそうでした。^[ツールがカバーする範囲が広く、自分の調べ方が良くなかったかもですが]

その中で以下の記事を見つけました。

https://tech.pepabo.com/2022/06/07/scenario-testing-in-go/

ひと目見てコレだ！という感じでした。  
もうこちらの記事と [REAMDE](https://github.com/k1LoW/runn) を読んで貰えれば大凡の使い勝手の理解が出来る可読性の高さがありました。  

サクッとプロジェクトに導入し、一部機能のコントリビュートしながら3ヶ月間触ってみて感じたことを記事にしたいと思います。

## APIチェーン楽チン

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
    desc: Updated User APIのテスト
    req:
      /user/{{ steps.createUser.res.body.id }}:
        put:
          body:
            application/json:
              username: "bob"
    test: steps.updateUser.res.status == 200
```

`{{ }}` で囲まれた `steps.createUser.res.body.id` の部分が `Create User` したレスポンスのIDを展開する式です。　　
実行したステップ名（上記例だと `createUser` と `updateUser` ) を指定して

* `ステップ名.res.status`  
レスポンスのステータスコード
* `ステップ名.res.headers['ヘッダー名'][0]`  
レスポンスヘッダー
* `ステップ名.res.body`  
レスポンスのボディ

が参照できます。
例えば以下のようなjsonレスポンスだった場合

```json
{
  "data": {
    "username": "alice",
    "email": "alice@example.com"
  }
}
```

`username` を参照するには `{{ ステップ名.res.body.data.username }}` で展開ができます。
期待値の検証も `test` 構文に同様に記載ができます。

```yml
    test: currect.res.status == 200 && currect.res.data.email == "alice@example.com"
```

ステップ名が `current` になっていますが、runner実行時と同じステップでtestを実行した場合にステップ名の予約後になっています。  
test構文の場合は `{{  }}` で式を囲まなく直接記載します。

## シナリオを再利用して書ける

varsとincludeを組み合わせてシナリオを再利用させることができます。  
前述例のAPIチェーンを再利用させて書くとこんな感じになります。

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
      desc: Create User APIのテスト（IDが発行される）
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

includeを使用してCreate Userのテストが再利用されるようになりました。  
include時にvarsを上書きできるようになっているので、必要なパラメータをvarsにしておくと再利用性が高まるかと思います。  
また `skipTest` を `true` にしておくと、includeされたシナリオのtest構文がスキップされるので軽微ですが実行時間の短縮になります。

注意点としてDRYに書きすぎると、どこまでも数珠つなぎになってしまいます。  
メンテナンス性や読みやすさを顧慮してシナリオを適度に共通化する感じが良いです。

## データ駆動テストができる

上記のincludeとvarsを組み合わせしたテストを発展させてデータ駆動テストっぽくシナリオを書いてみます。  
varsの設定でjson読み込みをサポートさせました。  

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
      desc: Create User APIのテスト（IDが発行される）
      req:
        /user:
          post:
            body:
              application/json: "{{ vars.createUserRequest }}"
      test: steps.createUser.res.status == 200  
  ```

リクエストbodyの内容をシナリオファイルから追い出すことができました。  
APIが複雑でパラメータが多い場合等、jsonデータをそのままリクエストで使えるようになるので大変便利です。  
レスポンスの期待値もjsonデータにしておくのもありです。  

もう少し発展させて、いくつかのデータパターンを繰り返しテストさせたくなります。  
[@k1low](https://twitter.com/k1low) さんと協議して runnに元々あったretry機能をloop機能として [リファクタリング＆機能拡張してもらい](https://github.com/k1LoW/runn/pull/97) 以下のようにすることができました。


* createUserBase.yml
  ```yml
  vars:
    createUserRequest: "json://path/to/user_request.json"
    createUserResponse: "json://path/to/user_response.json"
  steps:
    createUser:
      desc: Create User APIのテスト（IDが発行される）
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
      desc: Create User APIのテスト（IDが発行される）
      loop:
        count: len(vars.requestJson)
      include:
        path: createUserBase.yml
        vars:
          createUserRequest: "json://{{ vars.requestJson[i] }}"
          createUserResponse: "json://{{ vars.responseJson[i] }}"
  ```


createUserBase.ymlではレスポンスの期待値のチェック様に `compare` という組み込み関数を用意しました。  
もしレスポンス内で毎回値が変わるフィールドがあれば、第3引数以降にフィールド名を指定して除外できるようにもなっています。  

createUserPattern.ymlからcreateUserBase.ymlへリクエストと期待値のjsonをloop構文を使って渡しています。  
ループカウンターは `i` の部分になります。


## jsonファイル以外にも直接DBからデータを参照して活用できる

他のAPIツールにない協力な機能です。  
前述のloop構文と組み合わせればかなり強烈にアクセスができるかと思いますw

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

あと地味に登録系APIを呼び出しして、結果がレスポンスからは伺いしれないケースでもDBのレコード値を期待値として検証することができるのも大変役に立ちます。

## シナリオをシンプルにしデータは柔軟にする

データ駆動テストの例でもありますが、シナリオ自体はごくシンプルにしてデータに柔軟性をもたせることが重要かと考えています。  
試験的な機能ではあるのですが、[Go Template](https://pkg.go.dev/text/template)の機能を利用してデータを生成できる仕組みも組み込んでいます。

やり方は先程のvars構文でjsonを読み込む場合と同じで拡張子だけを `.json.template` にするだけです。  
includeのvarsのみで有効です。

* updateUserPattern.yml
  ```yml
    updatedUser:
      desc: Updated User APIのテスト
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

varsと変数展開と似ていて紛らわしいのですが、これはGo Templateの記法となります。  
こちらのテンプレート内でも前のステップのレスポンス等も参照できます。  
テンプレートならではな使い方として。ifやrangeが使えるのでイチからデータを作り込むことが可能になると考えています。  


## 色々な環境でもサクッと動かすことができる

goのコマンドとして実装されているので、CIやそれ以外の環境でも簡単に動かすことができます。
環境依存の部分は環境変数も参照可能なので切り替えが簡単にできます。

```yaml
runners:
  req:
    endpoint: ${END_POINT:-https://httpbin.org/}
  db: my:dbuser:${DB_PASS}@${DB_HOST:-localhost}:3306/dbname
```

runners構文でリクエスト先のエンドポイントや。DBの接続先を指定可能です。  
ここで環境変数を使えるので埋め込んでおくと良いです。  
またdocker-composeのようにdefault値も上述の様に記載ができるので普段はローカル用にして、それ以外の環境の場合は環境変数を上書きする感じで使います。

個人的に嬉しかった所として、デプロイ後の動作検証がrunnのコマンド一発で済むようになったことです。  

GitHub Actionsで簡単に動かせるようにカスタムアクションも用意したので良かったら是非！

https://github.com/k2tzumi/runn-action



## デバック実行とOpenAPIV3対応が親切

シナリオ作成時にはデバック実行させて行うのがベストです。  
デバック実行はrunn実行時のオプションで指定させるのが良いです。

```console
$ runn run path/to/scenario.yml --debug
```

リクエスト及びレスポンスの内容も全て表示されます。  
test構文でのチェックで字句解析されて結果が以下の様に出力されるので大変わかりやすいです。

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

またOpenAPIのv3にも対応していてSpecを以下のように指定しておくと幸せになれます。

```yaml
runners:
  req:
    endpoint: https://api.github.com
    openapi3: path/to/openapi.yaml
```

OpenAPIの仕様書通りのリクエストとレスポンスになってない場合はエラーにしてくれます。  
エラー内容も仕様書の定義を表示してくれるので、シナリオの記述間違いに気付くことができます。  
シナリオ自体の品質を高めるのと一緒にテスト対象のAPIの品質も同時にチェックできて大変良いです。  
実際にrunnに導入していくつかAPIの不具合を検知することが出来ました。

## 今後の発展性に期待！

今進行中なものとしてGolden Test用にCapture機能が実装され始めたり、組込み関数が実装しやすく拡張ポイントが提供されていたりもするので今後にも期待です。

## 最後に

素晴らしいツールを世に送り出してくれた [@k1low](https://twitter.com/k1low) さんに感謝です。  
記事を見かけて感じた、可読性の高さと学習の容易性は思惑以上でした。既にプロジェクト内には浸透済みとなり、なくてはならないツールになりました。  
この記事を見た方に是非試してみて頂きたいと思います。

また私個人（Contributor）としては、ふわっとした提案でも丁寧にディスカッションをして頂き大変ありがたかったです。  
背景や考え方についての学びも多く、何よりも発展的に機能が充実していく様を間近で見られるのが楽しいです。  
今後も微力ながら貢献できていけたらと思います。