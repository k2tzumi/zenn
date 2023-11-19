---
title: "【Day12】既存のシナリオから新しいシナリオを作成する / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-12 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/12配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして前半戦を終えようとしています。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[繰り返し処理を行ってみよう](https://zenn.dev/katzumi/articles/runn-tutorial-day11)」でした。

## 既存のシナリオから新しいシナリオを作成する

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day11) でステップを繰り返し実行できるようになりました。 
ここまでのチュートリアルで、ステップバイステップでシナリオを育てていった感じでした。

個人的にはunnの真骨頂だと思っている、発展的なシナリオ定義方法について説明をしていきたいと思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day12/list-articles.yml

まずはこちらのrunbookから。
特に以前の内容と変わりません。
前回までの記事の中で一番利用したステップです。こちらのステップを切り出した形です。

こちらのシナリオは単体で実行可能です。

```console
$  % USER=katzumi runn run day12/list-articles.yml        
.

1 scenario, 0 skipped, 0 failures
```

次にこちらのrunbookを利用して新しいシナリオを作成します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day12/include.yml

こちらのシナリオの内容は ["ステップに名前を付ける"](https://zenn.dev/katzumi/articles/runn-tutorial-day09#mapにする) の内容とまったく同じです。

主に違うのはここの部分です

https://github.com/k2tzumi/runn-tutorial/blob/main/day12/include.yml#L5-L7

`include` セクションで既存のrunbookのパスを指定するだけです。
これで `list-articles.yml` のシナリオ内容の続きから新しいステップを足して新しいシナリオが出来るようになります。

includeを使った場合の注意として、後続のステップではそのままinclude内のステップの結果を参照できないということです。

ここで重要になるのが、include先の

https://github.com/k2tzumi/runn-tutorial/blob/main/day12/list-articles.yml#L21-L22

こちらのbindです。
こちらのbindしたものが参照できます。
bindの使い方については ["任意の値に別名をつける"](https://zenn.dev/katzumi/articles/runn-tutorial-day10) をご参照ください。

include先のbindしたものを参照している2ステップ目（showFirstArticleステップ）

https://github.com/k2tzumi/runn-tutorial/blob/main/day12/include.yml#L8-L19

includeした場合の値の参照方法は以下のように変わります。

|内容|変更前|変更後|
|---|---|---|
|listArticlesステップの1番目の記事のslug|`steps.listArticles.res.body.articles[0].slug`|`steps.listArticles.articles[0].slug`|
|listArticlesステップの1番目の記事のID|`steps.listArticles.res.body.articles[0].id`|`steps.listArticles.articles[0].id`|

`steps.ステップ名.bindした別名` で参照出来るようになります。

3ステップ目（showSecondArticleステップ）も同じです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day12/include.yml#L20-L31

includeを使うことでステップの共通化することが出来ます。
またincludeを行ったrunbookも他のrunbookからincludeができます。
要は数珠つなぎにシナリオを作成することが出来ます！

実際に実行できるか？確認してみます。

```console
$ USER=katzumi runn run day12/include.yml
```
:::details 実行結果

```console
Run "指定された件数分、記事一覧を取得します" on "既存のシナリオから新しいシナリオを作成する".steps.listArticles
Run "指定された件数分、記事一覧を取得します" on "単体のシナリオとして定義".steps.listArticles
-----START HTTP REQUEST-----
GET /api/articles?count=2&order=daily&username=katzumi HTTP/1.1
Host: zenn.dev
Content-Type: application/json


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
Cache-Control: max-age=0, private, must-revalidate
Content-Type: application/json; charset=utf-8
Date: Sun, 19 Nov 2023 11:30:37 GMT
Etag: W/"02bb7d7a9eb0aaeb5e0b0032e87bcad5"
Referrer-Policy: strict-origin-when-cross-origin
Server: Google Frontend
Vary: Accept-Encoding, Origin
Via: 1.1 google
X-Cloud-Trace-Context: 7d59e26f62bd1a3d52bbc2bab3b5316f
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 30e7aab8-d802-4dea-8a40-46d63ee5375d
X-Runtime: 0.013630
X-Xss-Protection: 0

{"articles":[{"id":115084,"post_type":"Article","title":"APIシナリオテストの新ツールrunn","slug":"api-scenario-testing-with-runn","comments_count":0,"liked_count":172,"body_letters_count":10851,"article_type":"tech","emoji":"🧪","is_suspending_private":false,"published_at":"2022-09-20T00:29:27.454+09:00","body_updated_at":"2022-09-20T07:27:45.059+09:00","source_repo_updated_at":"2022-09-20T07:27:45.046+09:00","pinned":false,"path":"/katzumi/articles/api-scenario-testing-with-runn","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":118740,"post_type":"Article","title":"GitHub Actionsで連続pushした時に止めるアレ","slug":"using-concurrency-at-github-actions","comments_count":2,"liked_count":135,"body_letters_count":2018,"article_type":"tech","emoji":"⛔","is_suspending_private":false,"published_at":"2022-10-08T19:33:39.045+09:00","body_updated_at":"2022-10-08T19:33:39.045+09:00","source_repo_updated_at":"2022-10-08T19:33:39.039+09:00","pinned":false,"path":"/katzumi/articles/using-concurrency-at-github-actions","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null}],"next_page":2}
-----END HTTP RESPONSE-----
Run "bind" on "単体のシナリオとして定義".steps.listArticles
Run "test" on "単体のシナリオとして定義".steps.listArticles

Run "1番目の記事の詳細を取得します" on "既存のシナリオから新しいシナリオを作成する".steps.showFirstArticle
-----START HTTP REQUEST-----
GET /api/articles/api-scenario-testing-with-runn HTTP/1.1
Host: zenn.dev
Content-Type: application/json


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
Cache-Control: max-age=0, private, must-revalidate
Content-Type: application/json; charset=utf-8
Date: Sun, 19 Nov 2023 11:30:37 GMT
Etag: W/"eb0112b6b7179d5aeaf44702448f7aa8"
Referrer-Policy: strict-origin-when-cross-origin
Server: Google Frontend
Vary: Accept-Encoding, Origin
Via: 1.1 google
X-Cloud-Trace-Context: 649cb0d83793fd09dba375e6b705fe33
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 1a14bbe9-13e4-488e-99ca-4fe9cd962e19
X-Runtime: 0.025768
X-Xss-Protection: 0

{"article":{"id":115084, .. snip..  }}
-----END HTTP RESPONSE-----
Run "test" on "既存のシナリオから新しいシナリオを作成する".steps.showFirstArticle

Run "2番目の記事の詳細を取得します" on "既存のシナリオから新しいシナリオを作成する".steps.showSecondArticle
-----START HTTP REQUEST-----
GET /api/articles/using-concurrency-at-github-actions HTTP/1.1
Host: zenn.dev
Content-Type: application/json


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
Cache-Control: max-age=0, private, must-revalidate
Content-Type: application/json; charset=utf-8
Date: Sun, 19 Nov 2023 11:30:37 GMT
Etag: W/"8214658b9c4beb05b7e502a07c932d82"
Referrer-Policy: strict-origin-when-cross-origin
Server: Google Frontend
Vary: Accept-Encoding, Origin
Via: 1.1 google
X-Cloud-Trace-Context: b4e15ff45dff5ef4a9455ad8f8bd0d8a
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 42ca336a-0706-4aba-aac0-66120804aa71
X-Runtime: 0.033466
X-Xss-Protection: 0

{"article":{"id":118740, .. snip .. }}
-----END HTTP RESPONSE-----
Run "test" on "既存のシナリオから新しいシナリオを作成する".steps.showSecondArticle
.

1 scenario, 0 skipped, 0 failures
```
:::

今回2つのrunbookがあるので同時にテストを行う場合は以下のコマンドになります。

```console
$ USER=katzumi runn run day12/**/*.yml --verbose
=== 既存のシナリオから新しいシナリオを作成する (day12/include.yml) ... ok
=== 単体のシナリオとして定義 (day12/list-articles.yml) ... ok


2 scenarios, 0 skipped, 0 failures
```

ちょっとパスが特殊な書き方ですが、 `day12/**/*.yml` とすることで `day12` ディレクトリ配下の（ネストも含む）のrunbookが対象となります。
複数実行すると途中どこまで実行されたか分からなくなるので `--verbose` オプションを指定すると進捗がわかっていいです。

includeし過ぎはrunbookの依存関係が複雑になるので注意が必要ですが、効率よくシナリオを作成するには大変有効です。

明日は「includeしたシナリオの変数を書き換えよう」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day13