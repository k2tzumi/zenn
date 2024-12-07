---
title: "【Day11】繰り返し処理を行ってみよう / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","runn","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-11 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/11 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートして絶賛掲載中です。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[任意の値に別名をつける](https://zenn.dev/katzumi/articles/runn-tutorial-day10)」でした。

## 繰り返し処理を行ってみよう

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day10) で繰り返し利用する値に別名をつけられる様になりました。 
繰り返しでの参照はしやすくなりましたが、ステップ自体は手動で定義していました。今回はステップ自体も繰り返し実行する方法をご紹介します。


https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml

長いので 1st ビューに収ま【略】。

1 ステップ目までは前回までと同じです。違うのは 2 ステップ目と 3 ステップ目で、ステップを分けて繰り返し同じ API を実行していました。

メインの 2 ステップ目を見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L23-L35

こちらの内容で、すべての記事の詳細にアクセス出来るようになります。
今回の変数の count は 3 になっているので 3 件アクセスとなります。
以前の ["リクエストの結果を次のステップで利用してみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day08) では count を 1 件にして絞り込みを行っていました。

詳しく見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L25

この `loop` セクションで繰り返し実行数を指定します。
`len(articles)` で `articles` の件数になります。
急に出てきた `articles` ですが、1 つ前のステップで別名定義しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L21-L22

忘れていた方は ["任意の値に別名をつける"](https://zenn.dev/katzumi/articles/runn-tutorial-day10) を復習すると良いでしょう。

次に重要なのがこちら。

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L27

以前は `articles[0].slug` 若しくは `articles[1].slug` と手動で書き換えていました。
`i` はループ変数です。 `loop` セクションがあるステップでは自動で定義されます。
`articles[i].slug` で　`articles[0].slug` ... `articles[len(articles) - 1].slug` まで順次アクセスされるようになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L34-L35

もちろん `test` でもループ変数 `i` は使えます。

如何でしょうか？すごく簡単にループ処理も実現できました。

一点注意点として繰り返し実行すると DoS 攻撃になってしまいます。
開発中のテスト環境のサーバーであれば問題ないですが、外部サービス等へのアクセスは適切にインターバルを挟むようにしましょう。

詳細はオフィシャルのパラメータを参照してください。

https://github.com/k1LoW/runn#interval
https://github.com/k1LoW/runn#retry-runbook

明日は「既存のシナリオから新しいシナリオを作成する」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day12