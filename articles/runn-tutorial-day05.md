---
title: "【Day5】変数を使ってみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-05 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/05 配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日全部理解したら一人で runn を使って API シナリオテストや、ちょっとした API と連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は「[ステップに説明を付けてみる](https://zenn.dev/katzumi/articles/runn-tutorial-day04)」でした。

## 変数を使ってみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day04) でステップに説明を記載して可読性を上げる方法を説明しました。
また、HTTP Runner のステップ記述内容についても触れさせて頂きました。

今回はそれを発展させてシナリオを作る上で欠かせない変数の概念について説明したいと思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day05/vars.yml

少し記述が増えました。
Github Actions の workflow を記述したことがある人であれば、何となく読めるのではないでしょうか？

追加された箇所を順に説明していきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day05/vars.yml#L4-L5

`vars` セクションが追加されています。こちらのセクションで変数を定義しています。
`username` という変数名に `"zenn"` という *文字列* を定義しています。


https://github.com/k2tzumi/runn-tutorial/blob/main/day05/vars.yml#L9

変数を利用している箇所ですが、 `{{ vars.username }}` の箇所です。
定義した変数は `vars.変数名` で参照できます。ただそのまま記述してもエンドポイントのパスと区別がつかないので、`{{}}` で囲う必要があります。
`{{}}` で囲った内容は変数展開されると覚えておいてください。

昨日の例では username は固定で

`/api/articles?username=katzumi&order=latest`

というパスだったのが、変数展開をすると以下の様になります。

`/api/articles?username=zenn&order=latest`

:::details 実行結果
% runn run day05/vars.yml --debug 
Run "変数で指定されたユーザーの記事一覧を取得します" on "変数を使ってカスタマイズしやすくしましょう".steps[0]
-----START HTTP REQUEST-----
GET /api/articles?order=latest&username=zenn HTTP/1.1
Host: zenn.dev
Content-Type: application/json


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
Cache-Control: max-age=0, private, must-revalidate
Content-Type: application/json; charset=utf-8
Date: Sun, 19 Nov 2023 06:23:25 GMT
Etag: W/"c98fe9a80003cee44ef9bd34399347b5"
Referrer-Policy: strict-origin-when-cross-origin
Server: Google Frontend
Vary: Accept-Encoding, Origin
Via: 1.1 google
X-Cloud-Trace-Context: f267a7b46f1b6070b0ec8a69ebec6e3b
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 9ddcace6-ad13-4f11-a740-2d685d8b9a06
X-Runtime: 0.022112
X-Xss-Protection: 0

.. snip ..
-----END HTTP RESPONSE-----
.

1 scenario, 0 skipped, 0 failures
:::

変数を利用することでシナリオが柔軟に記述することが出来ます。  
また、変数は繰り返し扱うことも出来て、シナリオの意図がわかりやすくもなります。


明日は「変数を外部から与えててみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day06