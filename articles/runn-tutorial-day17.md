---
title: "【Day17】レスポンスをdumpして外部JSON化する / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー","runn"]
published: true
published_at: 2023-12-17 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/17 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートして絶賛掲載中です。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[JSONファイルにパラメータ埋め込みをしてみる](https://zenn.dev/katzumi/articles/runn-tutorial-day16)」でした。

## レスポンスをdumpして外部JSON化する

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day16) でリクエストの JSON にパラメータを埋め込む様にできました。 
今回は、リクエストだけでなくレスポンスを JSON 化させる方法を紹介していきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day17/dump-out.yml

レスポンスをまず取得するシナリオへ一時的にします。

https://github.com/k2tzumi/runn-tutorial/blob/main/day17/dump-out.yml#L21-L23

久しぶりに出てきました Dump Runner です。
（前回は ["Hello world!"](https://zenn.dev/katzumi/articles/runn-tutorial-day02) でした）

前回は 1 行に記載して標準出力されていましたが、今回は `expr` と `out` に分かれています。
`expr` は評価したい値を記載し、`out` には評価した値を保存するパスを相対パスで指定させます。

json の出力が終わったら、以下のように runbook を書き換えます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day17/dump-and-verify.yml

変更点はまず、レスポンスを JSON を読み込んでいます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day17/dump-and-verify.yml#L8

JSON ファイルの読み込みについては ["リクエストJSONを外部ファイル化する"](https://zenn.dev/katzumi/articles/runn-tutorial-day15) を参照ください。

次に dump を削除したかわりに test の条件を追加しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day17/dump-and-verify.yml#L22-L23

こちらの内容で JSON のファイルの内容とレスポンス BODY を比較することが出来ます。

実際に動作させてみましょう。


```console
$  USER=katzumi runn run day17/dump-and-verify.yml --verbose
=== dumpしたjsonを検証する (day17/dump-and-verify.yml) ... fail
    --- 指定された件数分、記事一覧を取得します (listArticles) ... fail
        Failure/Error: test failed on "dumpしたjsonを検証する".steps.listArticles "指定された件数分、記事一覧を取得します": condition is not true
        
        Condition:
          current.res.status == 200
          && len(current.res.body.articles) == vars.count
          && compare(current.res.body, vars.response)
          │
          ├── current.res.status => 200
          ├── 200 => 200
          ├── len(current.res.body.articles) => 2
          ├── current.res.body.articles => [{"article_type":"tech","body_letters_count":10851,"body_updated_at":"2022-09-20T07:27:45.059+09:00","comments_count":0,"emoji":"🧪","id":115084,"is_suspending_private":false,"liked_count":172,"path":"/katzumi/articles/api-scenario-testing-with-runn","pinned":false,"post_type":"Article","publication":null,"published_at":"2022-09-20T00:29:27.454+09:00","slug":"api-scenario-testing-with-runn","source_repo_updated_at":"2022-09-20T07:27:45.046+09:00","title":"APIシナリオテストの新ツールrunn","user":{"avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg","id":1578,"name":"katzumi","username":"katzumi"}},{"article_type":"tech","body_letters_count":2018,"body_updated_at":"2022-10-08T19:33:39.045+09:00","comments_count":2,"emoji":"⛔","id":118740,"is_suspending_private":false,"liked_count":136,"path":"/katzumi/articles/using-concurrency-at-github-actions","pinned":false,"post_type":"Article","publication":null,"published_at":"2022-10-08T19:33:39.045+09:00","slug":"using-concurrency-at-github-actions","source_repo_updated_at":"2022-10-08T19:33:39.039+09:00","title":"GitHub Actionsで連続pushした時に止めるアレ","user":{"avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg","id":1578,"name":"katzumi","username":"katzumi"}}]
          ├── vars.count => 2
          ├── compare(current.res.body, vars.response) => false
          ├── current.res.body => {"articles":[{"article_type":"tech","body_letters_count":10851,"body_updated_at":"2022-09-20T07:27:45.059+09:00","comments_count":0,"emoji":"🧪","id":115084,"is_suspending_private":false,"liked_count":172,"path":"/katzumi/articles/api-scenario-testing-with-runn","pinned":false,"post_type":"Article","publication":null,"published_at":"2022-09-20T00:29:27.454+09:00","slug":"api-scenario-testing-with-runn","source_repo_updated_at":"2022-09-20T07:27:45.046+09:00","title":"APIシナリオテストの新ツールrunn","user":{"avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg","id":1578,"name":"katzumi","username":"katzumi"}},{"article_type":"tech","body_letters_count":2018,"body_updated_at":"2022-10-08T19:33:39.045+09:00","comments_count":2,"emoji":"⛔","id":118740,"is_suspending_private":false,"liked_count":136,"path":"/katzumi/articles/using-concurrency-at-github-actions","pinned":false,"post_type":"Article","publication":null,"published_at":"2022-10-08T19:33:39.045+09:00","slug":"using-concurrency-at-github-actions","source_repo_updated_at":"2022-10-08T19:33:39.039+09:00","title":"GitHub Actionsで連続pushした時に止めるアレ","user":{"avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg","id":1578,"name":"katzumi","username":"katzumi"}}],"next_page":2}
          └── vars.response => {"articles":[{"article_type":"tech","body_letters_count":10851,"body_updated_at":"2022-09-20T07:27:45.059+09:00","comments_count":0,"emoji":"🧪","id":115084,"is_suspending_private":false,"liked_count":172,"path":"/katzumi/articles/api-scenario-testing-with-runn","pinned":false,"post_type":"Article","publication":null,"published_at":"2022-09-20T00:29:27.454+09:00","slug":"api-scenario-testing-with-runn","source_repo_updated_at":"2022-09-20T07:27:45.046+09:00","title":"APIシナリオテストの新ツールrunn","user":{"avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg","id":1578,"name":"katzumi","username":"katzumi"}},{"article_type":"tech","body_letters_count":2018,"body_updated_at":"2022-10-08T19:33:39.045+09:00","comments_count":2,"emoji":"⛔","id":118740,"is_suspending_private":false,"liked_count":134,"path":"/katzumi/articles/using-concurrency-at-github-actions","pinned":false,"post_type":"Article","publication":null,"published_at":"2022-10-08T19:33:39.045+09:00","slug":"using-concurrency-at-github-actions","source_repo_updated_at":"2022-10-08T19:33:39.039+09:00","title":"GitHub Actionsで連続pushした時に止めるアレ","user":{"avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg","id":1578,"name":"katzumi","username":"katzumi"}}],"next_page":2}
          


1 scenario, 0 skipped, 1 failure
```

エラーになりました。
前回取得していた JSON から何か結果が変わったようです。

JSON を再取得して実行し直してみました。

```console
$  USER=katzumi runn run day17/dump-out.yml --verbose 
=== レスポンスをdumpして外部json化する (day17/dump-out.yml) ... ok
    --- 指定された件数分、記事一覧を取得します (listArticles) ... ok


1 scenario, 0 skipped, 0 failures
$  USER=katzumi runn run day17/dump-and-verify.yml --verbose
=== dumpしたjsonを検証する (day17/dump-and-verify.yml) ... ok
    --- 指定された件数分、記事一覧を取得します (listArticles) ... ok


1 scenario, 0 skipped, 0 failures
```

今度は 2 つとも成功しました。

更新された articles.json は以下の様になります。

```console
$ git diff day17/articles.json
diff --git a/day17/articles.json b/day17/articles.json
index abe4a9e..9eca829 100644
--- a/day17/articles.json
+++ b/day17/articles.json
@@ -32,7 +32,7 @@
       "emoji": "⛔",
       "id": 118740,
       "is_suspending_private": false,
-      "liked_count": 134,
+      "liked_count": 136,
       "path": "/katzumi/articles/using-concurrency-at-github-actions",
       "pinned": false,
       "post_type": "Article",
```

`liked_count` が 2 つ増えていたようです。

如何でしたでしょうか？レスポンスも JSON 化して比較することが出来ました。
紹介した方法で JSON をテストに組み込むことができれば、前回のレスポンス結果から内容が変わった場合にすぐに気づくことが出来ます。
この様なテストは Golden Test と呼ばれます。API のデグレに気づく事ができますので、テクニックとして覚えておくと良いでしょう。


明日は「OpenAPI の仕様書通りか？テストする」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day18