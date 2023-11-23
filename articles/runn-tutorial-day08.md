---
title: "【Day8】リクエストの結果を次のステップで利用してみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-08 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/08配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[レスポンスの結果を検証してみる](https://zenn.dev/katzumi/articles/runn-tutorial-day07)」でした。

## リクエストの結果を次のステップで利用してみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day07) でレスポンス結果の参照及び検証を行う方法について説明しました。 
一つのエンドポイントに対して呼び出しと検証ができるようになったと思います。
ここからよりシナリオテストっぽくしていきたいと思います。

今までの内容ではステップが一つのみでしたが、今回始めて複数ステップになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day08/previous.yml

もうそろそろスクロールしないと見れないですねｗ
分割してみていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day08/previous.yml#L9-L19

一番目のステップです。
ここは前回と変わりません。

https://github.com/k2tzumi/runn-tutorial/blob/main/day08/previous.yml#L20-L30

今回のメインはこちらです。

詳しく見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day08/previous.yml#L22

`{{}}` の部分で変数展開されているのがわかると思います。
以前の ["変数を使ってみる"](http://localhost:8000/articles/runn-tutorial-day05)での説明や今回の1番目のステップの様に `vars.変数名` ではなく `previous.resp.body` という参照になっているかと思います。
["レスポンスの結果を検証してみる"](http://localhost:8000/articles/runn-tutorial-day07) で説明した `current.res.body` と近い記述になっているかと思います。
違いは `current` が `previous` になっているだけです。
`previous` は一つ前のステップを表すものになります。逆に？ `current` が現在のステップを表すものになります。
これらはrunnが自動的に作成する変数となります。

今回のケースでは `previous.res.body.articles[0].slug` となっており、記事一覧取得のレスポンスで一番目の記事（配列は0から始まります）のslugにアクセスしています。
`slug` はJSONのキーの名前ですが、一般的にはURLのパスの一部になります。

https://developer.mozilla.org/ja/docs/Glossary/Slug

動きを理解する為に実際にdebug実行させてみましょう。
今回は変数でcoutは1件のみに絞っています。

```console
% USER=katzumi runn run day08/previous.yml --debug
Run "指定された件数分、記事一覧を取得します" on "次のステップで一つ前のレスポンスを引き継いでみよう".steps[0]
-----START HTTP REQUEST-----
GET /api/articles?count=1&order=daily&username=katzumi HTTP/1.1
Host: zenn.dev
Content-Type: application/json


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
Cache-Control: max-age=0, private, must-revalidate
Content-Type: application/json; charset=utf-8
Date: Sun, 19 Nov 2023 08:32:20 GMT
Etag: W/"03bc53e4ed94561f41ca79dea5359c00"
Referrer-Policy: strict-origin-when-cross-origin
Server: Google Frontend
Vary: Accept-Encoding, Origin
Via: 1.1 google
X-Cloud-Trace-Context: 2bd7a1c4a89563bafcb968a57420ab6d
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 95248596-93ae-4ba4-a45c-c6c3f4fc2cb9
X-Runtime: 0.014307
X-Xss-Protection: 0

{"articles":[{"id":115084,"post_type":"Article","title":"APIシナリオテストの新ツールrunn","slug":"api-scenario-testing-with-runn","comments_count":0,"liked_count":172,"body_letters_count":10851,"article_type":"tech","emoji":"🧪","is_suspending_private":false,"published_at":"2022-09-20T00:29:27.454+09:00","body_updated_at":"2022-09-20T07:27:45.059+09:00","source_repo_updated_at":"2022-09-20T07:27:45.046+09:00","pinned":false,"path":"/katzumi/articles/api-scenario-testing-with-runn","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null}],"next_page":2}
-----END HTTP RESPONSE-----
Run "test" on "次のステップで一つ前のレスポンスを引き継いでみよう".steps[0]

Run "記事の詳細を取得します" on "次のステップで一つ前のレスポンスを引き継いでみよう".steps[1]
-----START HTTP REQUEST-----
GET /api/articles/api-scenario-testing-with-runn HTTP/1.1
Host: zenn.dev
Content-Type: application/json


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK

.. snip ..

-----END HTTP RESPONSE-----
Run "test" on "次のステップで一つ前のレスポンスを引き継いでみよう".steps[1]
.

1 scenario, 0 skipped, 0 failures
```

1ステップ目のレスポンスのJSONを整形すると以下の様になっていました。

```json
{
  "articles": [
    {
      "id": 115084,
      "post_type": "Article",
      "title": "APIシナリオテストの新ツールrunn",
      "slug": "api-scenario-testing-with-runn",
      "comments_count": 0,
      "liked_count": 172,
      "body_letters_count": 10851,
      "article_type": "tech",
      "emoji": "🧪",
      "is_suspending_private": false,
      "published_at": "2022-09-20T00:29:27.454+09:00",
      "body_updated_at": "2022-09-20T07:27:45.059+09:00",
      "source_repo_updated_at": "2022-09-20T07:27:45.046+09:00",
      "pinned": false,
      "path": "/katzumi/articles/api-scenario-testing-with-runn",
      "user": {
        "id": 1578,
        "username": "katzumi",
        "name": "katzumi",
        "avatar_small_url": "https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"
      },
      "publication": null
    }
  ],
  "next_page": 2
}
```

こちらの `slug` が `api-scenario-testing-with-runn` となっています。
こちらの変数が展開されており2件目のステップが

```console
Run "記事の詳細を取得します" on "次のステップで一つ前のレスポンスを引き継いでみよう".steps[1]
-----START HTTP REQUEST-----
GET /api/articles/api-scenario-testing-with-runn HTTP/1.1
Host: zenn.dev
Content-Type: application/json
```

上記のようなアクセスになっています。
ステップが分かれるとdescで指定した内容がでるのでわかりやすくなると思います。descの書き方は ["ステップに説明を付けてみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day04) を参照ください。

こちらの一連のステップで記事一覧から1番目の記事の詳細を取得するというシナリオが実現できています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day08/previous.yml#L26-L30

最後にテストも見てみましょう。
ここでも `previous` を利用します。
記事の詳細はslugでアクセスしていますが、テストではidの比較を行っています。

* `current.res.body.article.id` 
現在のステップの記事のID
* `previous.res.body.articles[0].id`  
前回のステップ（記事一覧取得）の記事リストの最初の記事のID（記事詳細取得時のID）

という意味になります。
こうすることでより厳密なテストになることでしょう。

如何でしたでしょうか？複数のエンドポイントのAPIを組み合わせてのテストが非常に簡単に行えることが理解頂けたことと思います！

明日以降ももっと便利に使えるになるチュートリアルをご紹介しますのでお楽しみに

明日は「ステップに名前を付ける」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day09