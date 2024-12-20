---
title: "【Day4】ステップに説明を付けてみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","runn","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-04 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/04 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[curlコマンドをreplayさせる](https://zenn.dev/katzumi/articles/runn-tutorial-day03)」でした。

## ステップに説明を付けてみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day03) で cURL のコマンドを ruun のシナリオに変換することを行いました。 
ただそのままだと何の API を呼び出しているのか？人にはわかりません。
シナリオを文章化する意味でもステップに説明をつけていきましょう。

前回の runbook は長いので一部割愛してシンプル化も合わせてします。

https://github.com/k2tzumi/runn-tutorial/blob/main/day04/desc.yml

runbook 全体の他にステップ内にも `desc` セクションで説明を追加することが出来ます。
これでぐっと可読性が高くなります。
こちらの説明は debug 実行時にも出力され、何をやっているのか？わかりやすくなり、エラー発生時にもステップを特定しやすくなります。

```console
$ runn run day04/desc.yml --debug             
Run "katzumiの記事一覧を取得します" on "Descriptionをつけて見やすくしましょう".steps[0]
-----START HTTP REQUEST-----
GET /api/articles?order=latest&username=katzumi HTTP/1.1
Host: zenn.dev
Content-Type: application/json
```

わかりやすいように前回の内容との比較してみます。

```console
$ runn run day03/curl.yml --debug 
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
```

いかがでしょうか？

## 前日のおさらい

前回の記事では出力された runbook の説明を割愛してしまったので、シンプルになった runbook で説明をします。

最初に `runners` というセクションが増えています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day04/desc.yml#L2-L3

こちらのセクションでは、runbook 内で実行する各種 Runner を定義します。
こちらの `req` という名前で定義されているものが HTTP Runner になります。`http://` と `https://` のプロトコル名から始める必要があります。  
後日触れますが、http 以外のプロトコルの Runner が幾つか用意されています。
こちらのセクションでそれらの Runner の名前とオプションを定義する箇所だと覚えておいて貰えれば OK です。


https://github.com/k2tzumi/runn-tutorial/blob/main/day04/desc.yml#L6-L10

次にリクエストを送信している部分になります。
こちらの `req` は先程の説明の `runners` セクションで定義された Runner の名前になります。
今回は、 `req` のエンドポイントが `https://zenn.dev` になっており、こちらの Runner を使ったアクセスはすべて　`https://zenn.dev` へのリクエストとなります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day04/desc.yml#L7

`req`（Runner セクション）の次の階層に指定するのがリクエストのパスになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day04/desc.yml#L8

リクエストパスの下の階層が　HTTP リクエストのメソッドを指定します。こちらが `get` になっており、今回は GET メソッドでリクエストすることになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day04/desc.yml#L9-L10

HTTP メソッドの下の改装にリクエスト内容を定義します。今回は request body を指定しています。

:::details リクエストヘッダーも定義できます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day03/curl.yml#L8-L23

:::

HTTP 通信の理解があれば、この内容を理解するのは容易でしょう。


明日は「変数を使ってみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day05