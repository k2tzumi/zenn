---
title: "【Day22】CIに組み込んでみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー","runn"]
published: true
published_at: 2023-12-22 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/22 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートしてそろそろラストスパートです。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[CDPを使ってブラウザ操作してみる](https://zenn.dev/katzumi/articles/runn-tutorial-day21)」でした。

## CIに組み込んでみる

今まで色々なシナリオを作成していきましたが、自動テストとして組み込みたくなります。
今日は runn を GitHub Acitons の workflow として実行してみたいきます。


https://github.com/k2tzumi/runn-tutorial/blob/main/day22/github-action.yml

今回の API は以下を利用したいきます。

https://httpbin.org/

こちらは色々な HTTP メソッドを試せるサービスで実行環境が Docker イメージとしても公開されています。

runn を CI で動かす際に重要となる所が以下の箇所となります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day22/github-action.yml#L3-L4

runners の各 Runner で環境依存のものは環境変数にすることです。
CI 上のサービスと連携してテストすることになりますので、API のエンドポイントなどを変更できるようにします。

:::details ローカル実行結果。

```console
% runn run day22/github-action.yml --debug
Run "Specify request contents by converting variables with built-in functions" on "Github Actionsから実行する（環境依存のパラメータは環境変数を利用する）".steps[0]
-----START HTTP REQUEST-----
GET /get?var=C%2B%2B HTTP/1.1
Host: httpbin.org


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 306
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Thu, 23 Nov 2023 11:41:37 GMT
Server: gunicorn/19.9.0

{
  "args": {
    "var": "C++"
  }, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-655f3a71-7a4f91da3af4dcf849370d9d"
  }, 
  "origin": "192.168.0.1", 
  "url": "https://httpbin.org/get?var=C%2B%2B"
}

-----END HTTP RESPONSE-----
Run "test" on "Github Actionsから実行する（環境依存のパラメータは環境変数を利用する）".steps[0]

Run "Specify variables in the request body" on "Github Actionsから実行する（環境依存のパラメータは環境変数を利用する）".steps[1]
-----START HTTP REQUEST-----
POST /post HTTP/1.1
Host: httpbin.org
Content-Type: application/json

{"bar":1,"foo":"test"}
-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 470
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Thu, 23 Nov 2023 11:41:38 GMT
Server: gunicorn/19.9.0

{
  "args": {}, 
  "data": "{\"bar\":1,\"foo\":\"test\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "22", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-655f3a71-30bf7bcf78dd2f8d2855c593"
  }, 
  "json": {
    "bar": 1, 
    "foo": "test"
  }, 
  "origin": "192.168.0.1", 
  "url": "https://httpbin.org/post"
}

-----END HTTP RESPONSE-----
Run "test" on "Github Actionsから実行する（環境依存のパラメータは環境変数を利用する）".steps[1]

Run "Specify array variables in the request body" on "Github Actionsから実行する（環境依存のパラメータは環境変数を利用する）".steps[2]
-----START HTTP REQUEST-----
PUT /put HTTP/1.1
Host: httpbin.org
Content-Type: application/json

[{"bar":1,"foo":"test1"},{"bar":2,"foo":"test2"}]
-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 571
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Thu, 23 Nov 2023 11:41:38 GMT
Server: gunicorn/19.9.0

{
  "args": {}, 
  "data": "[{\"bar\":1,\"foo\":\"test1\"},{\"bar\":2,\"foo\":\"test2\"}]", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "49", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-655f3a72-25d6f4240c46ddea4fa68b0f"
  }, 
  "json": [
    {
      "bar": 1, 
      "foo": "test1"
    }, 
    {
      "bar": 2, 
      "foo": "test2"
    }
  ], 
  "origin": "192.168.0.1", 
  "url": "https://httpbin.org/put"
}

-----END HTTP RESPONSE-----
Run "test" on "Github Actionsから実行する（環境依存のパラメータは環境変数を利用する）".steps[2]
.

1 scenario, 0 skipped, 0 failures
```

:::

ローカルで問題なく動いたので GitHub Actions の workflow を見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/ci.yml

要点だけ説明します。

https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/ci.yml#L16-L20

こちらでテスト対象のサービスを立ち上げています。
今回はコンテナイメージがそのまま使えましたが、なかなか外部公開されているサービスをテストすることはないと考えます。
workflow 内でサーバーを立ち上げるように実行していく形になります。


https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/ci.yml#L24-L29

こちらが runn の実行本体になります。
カスタムアクションが用意されていますので、こちらの記述の様に指定すれば最低限動きます。
あと、環境依存の部分は `env` で環境変数として注入してあげれば OK です。

カスタムアクションの詳細（各種パラメータについて）はこちらのページをご参照ください。
https://github.com/marketplace/actions/run-runn

もしパラメータの追加等の要望があれば、 [こちら](https://github.com/k2tzumi/runn-tutorial/issues/new) よりご依頼ください。

workflow の実行時の例としては以下のようになります。

https://github.com/k2tzumi/runn-tutorial/actions/runs/6968579651/job/18962718606#step:5:14

如何でしたでしょうか？runn は golang のアプリケーションはワンバイナリで動くので CI 上での実行も簡単です。
なかなか CI 上で E2E テストを組み込むのはハードルが高い印象でしたが、runn による API テストでは容易に組み込むことが出来るのでオススメです。
ミディアムテストとして効率よくカバレッジを稼ぐことができ、ブラウザテストに比べて安定してスピードも早いのでコスパが良いです。

明日は「ファイルをアップロードする」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day23