---
title: "curlコマンドをreplayさせる"
free: true
---

# curlコマンドをreplayさせる

今回は手軽に runn を始める際に便利な使い方を紹介します。  
curl コマンドを叩いて API のテストをすることは、この記事を読んでいる方なら一度は経験があるでしょう 
毎回 curl のコマンドを書くのは面倒くさいので、それを runbook にします。

説明の為に最初は普通に curl を実行してみます。

以下のコマンドは zenn.dev にアクセスして Developer Tool から取得してきた curl コマンドです。
```console
$ curl 'https://zenn.dev/api/articles?username=katzumi&count=96&order=latest' \
  -H 'authority: zenn.dev' \
  -H 'accept: */*' \
  -H 'accept-language: ja,en-US;q=0.9,en;q=0.8,ko;q=0.7' \
  -H 'content-type: application/json' \
  -H 'if-none-match: W/"9b385c38c8f9e339034c678157cd50a5"' \
  -H 'referer: https://zenn.dev/katzumi' \
  -H 'sec-ch-ua: "Google Chrome";v="119", "Chromium";v="119", "Not?A_Brand";v="24"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-origin' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36' \
  --compressed
```

<!-- textlint-disable -->
:::details 実行結果
```json
{"articles":[{"id":209766,"post_type":"Article","title":"PHP Conference Japan 2023 参加レポート","slug":"php-conference-japan-2023","comments_count":0,"liked_count":2,"body_letters_count":4454,"article_type":"idea","emoji":"♨","is_suspending_private":false,"published_at":"2023-10-15T16:55:28.647+09:00","body_updated_at":"2023-10-15T22:06:15.918+09:00","source_repo_updated_at":"2023-10-15T22:06:15.910+09:00","pinned":false,"path":"/katzumi/articles/php-conference-japan-2023","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":208493,"post_type":"Article","title":"PHP Conference Japan 2023 スライドまとめ","slug":"php-conference-japan-2023-slides-link","comments_count":0,"liked_count":31,"body_letters_count":16734,"article_type":"idea","emoji":"🐘","is_suspending_private":false,"published_at":"2023-10-10T01:58:55.412+09:00","body_updated_at":"2023-10-25T20:47:10.499+09:00","source_repo_updated_at":"2023-10-25T20:47:10.494+09:00","pinned":false,"path":"/katzumi/articles/php-conference-japan-2023-slides-link","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":206442,"post_type":"Article","title":"推し OSS に貢献して T シャツをもらおう【タイトル誤り訂正あり〼】","slug":"hacktoberfest-2023","comments_count":0,"liked_count":0,"body_letters_count":7624,"article_type":"idea","emoji":"👕","is_suspending_private":false,"published_at":"2023-09-30T21:14:53.693+09:00","body_updated_at":"2023-10-07T10:24:25.578+09:00","source_repo_updated_at":"2023-10-07T10:24:25.572+09:00","pinned":false,"path":"/katzumi/articles/hacktoberfest-2023","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":204974,"post_type":"Article","title":"PHP カンファレンス沖縄 2023 で登壇してきました","slug":"phpcon-okinawa-2023","comments_count":0,"liked_count":4,"body_letters_count":3557,"article_type":"idea","emoji":"🌺","is_suspending_private":false,"published_at":"2023-09-24T18:51:09.776+09:00","body_updated_at":"2023-09-24T18:51:09.776+09:00","source_repo_updated_at":"2023-09-24T18:51:09.776+09:00","pinned":false,"path":"/katzumi/articles/phpcon-okinawa-2023","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":182186,"post_type":"Article","title":"PHP カンファレンス福岡 2023 で登壇してきました（Jun 22-25）","slug":"phpcon-fukuoka-2023-talk-impression","comments_count":1,"liked_count":7,"body_letters_count":8612,"article_type":"idea","emoji":"🍲","is_suspending_private":false,"published_at":"2023-07-02T19:48:33.136+09:00","body_updated_at":"2023-07-02T19:48:33.136+09:00","source_repo_updated_at":"2023-07-02T19:48:33.136+09:00","pinned":false,"path":"/katzumi/articles/phpcon-fukuoka-2023-talk-impression","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":158399,"post_type":"Article","title":"runn 開発者会議 in PHPerKaigi 2023","slug":"runn-developers-conference-in-phperkaigi2023","comments_count":0,"liked_count":5,"body_letters_count":4861,"article_type":"idea","emoji":"🐘","is_suspending_private":false,"published_at":"2023-03-28T20:26:07.554+09:00","body_updated_at":"2023-03-28T20:26:07.554+09:00","source_repo_updated_at":"2023-03-28T20:26:07.554+09:00","pinned":false,"path":"/katzumi/articles/runn-developers-conference-in-phperkaigi2023","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":154668,"post_type":"Article","title":"アプリ内の function を GAS ライブラリからコールバックさせる方法","slug":"gas-library-globalthis-scope","comments_count":0,"liked_count":1,"body_letters_count":2766,"article_type":"tech","emoji":"📞","is_suspending_private":false,"published_at":"2023-03-12T18:21:31.913+09:00","body_updated_at":"2023-03-12T18:21:31.913+09:00","source_repo_updated_at":"2023-03-12T18:21:31.913+09:00","pinned":false,"path":"/katzumi/articles/gas-library-globalthis-scope","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":151503,"post_type":"Article","title":"App manifest で Slack アプリを簡単インストール＆GPT チャットボット体験","slug":"introduction-of-slack-bot-with-app-manifest","comments_count":0,"liked_count":4,"body_letters_count":11066,"article_type":"tech","emoji":"📦","is_suspending_private":false,"published_at":"2023-02-26T21:32:32.613+09:00","body_updated_at":"2023-02-26T21:32:32.613+09:00","source_repo_updated_at":"2023-02-26T21:32:32.613+09:00","pinned":false,"path":"/katzumi/articles/introduction-of-slack-bot-with-app-manifest","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":133205,"post_type":"Article","title":"スキーマ駆動開発を支える開発プロセス＆ツール群","slug":"re-architecting-rezept2","comments_count":0,"liked_count":9,"body_letters_count":6990,"article_type":"tech","emoji":"🕹️","is_suspending_private":false,"published_at":"2022-12-15T00:00:01.050+09:00","body_updated_at":"2022-12-15T00:00:01.050+09:00","source_repo_updated_at":"2022-12-14T20:24:40.861+09:00","pinned":false,"path":"/katzumi/articles/re-architecting-rezept2","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":133204,"post_type":"Article","title":"ビジネス変化に適用する為にリアーキテクティングを行うまでの話","slug":"re-architecting-rezept","comments_count":0,"liked_count":9,"body_letters_count":5288,"article_type":"idea","emoji":"🧬","is_suspending_private":false,"published_at":"2022-12-14T00:00:00.645+09:00","body_updated_at":"2022-12-15T08:30:26.831+09:00","source_repo_updated_at":"2022-12-15T08:30:26.823+09:00","pinned":false,"path":"/katzumi/articles/re-architecting-rezept","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":127547,"post_type":"Article","title":"BaaS なプロジェクトに tagpr を導入してみた","slug":"introducing-tagpr-to-backend-as-a-service","comments_count":0,"liked_count":10,"body_letters_count":5344,"article_type":"tech","emoji":"🏷️","is_suspending_private":false,"published_at":"2022-11-22T00:35:32.946+09:00","body_updated_at":"2022-11-22T00:35:32.946+09:00","source_repo_updated_at":"2022-11-22T00:35:32.942+09:00","pinned":false,"path":"/katzumi/articles/introducing-tagpr-to-backend-as-a-service","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":127400,"post_type":"Article","title":"Twitter のシステム構成図をまとめてみる","slug":"twitter-whiteboard","comments_count":0,"liked_count":3,"body_letters_count":3003,"article_type":"tech","emoji":"🕊️","is_suspending_private":false,"published_at":"2022-11-20T10:54:36.629+09:00","body_updated_at":"2022-11-20T14:49:04.707+09:00","source_repo_updated_at":"2022-11-20T14:49:04.702+09:00","pinned":false,"path":"/katzumi/articles/twitter-whiteboard","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":123212,"post_type":"Article","title":"実装と乖離させないスキーマ駆動開発フロー / OpenAPI Laravel 編","slug":"schema-driven-development-flow","comments_count":0,"liked_count":19,"body_letters_count":16209,"article_type":"tech","emoji":"📐","is_suspending_private":false,"published_at":"2022-10-31T06:30:18.671+09:00","body_updated_at":"2022-10-31T13:27:11.679+09:00","source_repo_updated_at":"2022-10-31T13:27:11.674+09:00","pinned":false,"path":"/katzumi/articles/schema-driven-development-flow","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":118740,"post_type":"Article","title":"GitHub Actions で連続 push した時に止めるアレ","slug":"using-concurrency-at-github-actions","comments_count":2,"liked_count":135,"body_letters_count":2018,"article_type":"tech","emoji":"⛔","is_suspending_private":false,"published_at":"2022-10-08T19:33:39.045+09:00","body_updated_at":"2022-10-08T19:33:39.045+09:00","source_repo_updated_at":"2022-10-08T19:33:39.039+09:00","pinned":false,"path":"/katzumi/articles/using-concurrency-at-github-actions","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":115710,"post_type":"Article","title":"PHPUnit でカバレッジがうまく取れなくて嵌った話","slug":"apologies-for-phpunit-coverage","comments_count":0,"liked_count":8,"body_letters_count":2855,"article_type":"tech","emoji":"🙇","is_suspending_private":false,"published_at":"2022-09-22T20:23:04.526+09:00","body_updated_at":"2022-09-22T20:23:04.526+09:00","source_repo_updated_at":"2022-09-22T20:31:59.519+09:00","pinned":false,"path":"/katzumi/articles/apologies-for-phpunit-coverage","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":115084,"post_type":"Article","title":"API シナリオテストの新ツール runn","slug":"api-scenario-testing-with-runn","comments_count":0,"liked_count":172,"body_letters_count":10851,"article_type":"tech","emoji":"🧪","is_suspending_private":false,"published_at":"2022-09-20T00:29:27.454+09:00","body_updated_at":"2022-09-20T07:27:45.059+09:00","source_repo_updated_at":"2022-09-20T07:27:45.046+09:00","pinned":false,"path":"/katzumi/articles/api-scenario-testing-with-runn","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":64693,"post_type":"Article","title":"法改正をマイクロサービスで立ち向かう（後編）","slug":"confronting-law-amends-with-microservices2","comments_count":0,"liked_count":7,"body_letters_count":7014,"article_type":"idea","emoji":"🙆","is_suspending_private":false,"published_at":"2021-12-24T08:21:17.697+09:00","body_updated_at":"2021-12-24T08:21:17.697+09:00","source_repo_updated_at":"2021-12-24T08:21:17.697+09:00","pinned":false,"path":"/katzumi/articles/confronting-law-amends-with-microservices2","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":47863,"post_type":"Article","title":"LGTM 画像を作成する Slack App を作ったお話","slug":"0e9c0a49258afc","comments_count":0,"liked_count":3,"body_letters_count":3381,"article_type":"tech","emoji":"🖼️","is_suspending_private":false,"published_at":"2021-12-21T15:46:52.998+09:00","body_updated_at":"2021-12-21T15:46:52.998+09:00","source_repo_updated_at":null,"pinned":false,"path":"/katzumi/articles/0e9c0a49258afc","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":62087,"post_type":"Article","title":"法改正をマイクロサービスで立ち向かう（前編）","slug":"confronting-law-amends-with-microservices","comments_count":1,"liked_count":11,"body_letters_count":5639,"article_type":"idea","emoji":"🙆","is_suspending_private":false,"published_at":"2021-12-15T00:13:27.703+09:00","body_updated_at":"2021-12-24T19:26:39.872+09:00","source_repo_updated_at":"2021-12-24T19:26:39.872+09:00","pinned":false,"path":"/katzumi/articles/confronting-law-amends-with-microservices","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":45167,"post_type":"Article","title":"Google Apps Script 向け簡易 job-queue を作った","slug":"58354fb4d05038","comments_count":0,"liked_count":11,"body_letters_count":4914,"article_type":"tech","emoji":"💼","is_suspending_private":false,"published_at":"2021-08-01T17:46:58.029+09:00","body_updated_at":"2021-08-01T17:46:58.029+09:00","source_repo_updated_at":null,"pinned":false,"path":"/katzumi/articles/58354fb4d05038","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":12666,"post_type":"Article","title":"リモートモブプログラミングを 9 ヶ月間やってきたので振り返ってみる","slug":"36bdc74887543c","comments_count":2,"liked_count":24,"body_letters_count":9070,"article_type":"idea","emoji":"👨‍👩‍👧","is_suspending_private":false,"published_at":"2020-12-20T23:05:44.854+09:00","body_updated_at":"2020-12-20T23:05:44.854+09:00","source_repo_updated_at":null,"pinned":false,"path":"/katzumi/articles/36bdc74887543c","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null},{"id":11812,"post_type":"Article","title":"GitHub Actions でカバレッジを可視化する","slug":"995df5abebc91e312167","comments_count":0,"liked_count":39,"body_letters_count":11233,"article_type":"tech","emoji":"👷","is_suspending_private":false,"published_at":"2020-12-08T18:30:52.811+09:00","body_updated_at":"2020-12-13T11:18:58.562+09:00","source_repo_updated_at":null,"pinned":false,"path":"/katzumi/articles/995df5abebc91e312167","user":{"id":1578,"username":"katzumi","name":"katzumi","avatar_small_url":"https://res.cloudinary.com/zenn/image/fetch/s--O0GLM2Pk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_70/https://storage.googleapis.com/zenn-user-upload/avatar/8fb0c88e62.jpeg"},"publication":null}],"next_page":null}
```
:::
<!-- textlint-enable -->

json がコマンドの出力結果として表示されれば OK です。
上記コマンドの前に `runn new -` を付け加えて以下のように変更します。

```console
$ runn new -- curl 'https://zenn.dev/api/articles?username=katzumi&count=96&order=latest' \
  -H 'authority: zenn.dev' \
  -H 'accept: */*' \
  -H 'accept-language: ja,en-US;q=0.9,en;q=0.8,ko;q=0.7' \
  -H 'content-type: application/json' \
  -H 'if-none-match: W/"9b385c38c8f9e339034c678157cd50a5"' \
  -H 'referer: https://zenn.dev/katzumi' \
  -H 'sec-ch-ua: "Google Chrome";v="119", "Chromium";v="119", "Not?A_Brand";v="24"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-origin' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36' \
  --compressed
```

<!-- textlint-disable -->
:::details 実行結果
desc: Generated by `runn new`
runners:
  req: https://zenn.dev
steps:
- req:
    /api/articles?username=katzumi&count=96&order=latest:
      get:
        headers:
          Accept: '*/*'
          Accept-Encoding: deflate, gzip
          Accept-Language: ja,en-US;q=0.9,en;q=0.8,ko;q=0.7
          Authority: zenn.dev
          If-None-Match: W/"9b385c38c8f9e339034c678157cd50a5"
          Referer: https://zenn.dev/katzumi
          Sec-Ch-Ua: '"Google Chrome";v="119", "Chromium";v="119", "Not?A_Brand";v="24"'
          Sec-Ch-Ua-Mobile: ?0
          Sec-Ch-Ua-Platform: '"macOS"'
          Sec-Fetch-Dest: empty
          Sec-Fetch-Mode: cors
          Sec-Fetch-Site: same-origin
          User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36
            (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36
        body:
          application/json: null
:::
<!-- textlint-enable -->

上記スクリプトをコピーするか、先程のコマンドの出力結果をリダイレクトしてファイルを取得すると以下の様になります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day03/curl.yml


もちろん実行可能です。

```console
$ runn run day03/curl.yml
.

1 scenario, 0 skipped, 0 failures
```

簡単過ぎて動いているか不安なのでデバック実行させてみます。 `--debug` オプションを追加します。
長いのとレスポンスにバイナリが含まれていますので一部割愛しています。

```console
$ % runn run day03/curl.yml --debug  
Run "req" on "Generated by `runn new`".steps[0]
-----START HTTP REQUEST-----
GET /api/articles?count=96&order=latest&username=katzumi HTTP/1.1
Host: zenn.dev
Accept: */*
Accept-Encoding: deflate, gzip
Accept-Language: ja,en-US;q=0.9,en;q=0.8,ko;q=0.7
Authority: zenn.dev
Cache-Control: no-cache
Content-Type: application/json
Pragma: no-cache
Referer: https://zenn.dev/katzumi
Sec-Ch-Ua: Chromium";v="118", "Google Chrome";v="118", "Not=A?Brand";v="99
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: macOS
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 2966
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
Cache-Control: max-age=0, private, must-revalidate
Content-Encoding: gzip
Content-Type: application/json; charset=utf-8
Date: Sun, 19 Nov 2023 05:21:52 GMT
Etag: W/"5374487d3953b6d53ca6f01e87710398"
Referrer-Policy: strict-origin-when-cross-origin
Server: Google Frontend
Vary: Accept, Accept-Encoding, Origin
Via: 1.1 google
X-Cloud-Trace-Context: d8d7745314303bc5e389f1500e23dd71
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 75a2cf49-5452-4ddd-a2e0-1c3550d72f70
X-Runtime: 0.054394
X-Xss-Protection: 0

p?Ye?]s?Vǿ??3{?H>z?c{A??N3??jg'#˲-?X?^`i?3H?4$?	??M?BX??,?h????`;??a?#?	?-Kv?;?d[?#ۏ???T?\??-?+????hV?*?IO?????QT???'O}ӷȑ???

... snip ..

-----END HTTP RESPONSE-----
.

1 scenario, 0 skipped, 0 failures
```

こちらの内容をみると、動作が正常に行われていることが明確にわかります。

この手順を覚えさえすれば、runn での自動化の第一歩を踏み出すことが出来ます！
