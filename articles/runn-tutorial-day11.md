---
title: "【Day11】繰り返し処理を行ってみよう / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-11 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/11配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして絶賛掲載中です。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[任意の値に別名をつける](https://zenn.dev/katzumi/articles/runn-tutorial-day10)」でした。

## 繰り返し処理を行ってみよう

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day10) で繰り返し利用する値に別名をつけれる様になりました。 
繰り返しでの参照はしやすくなりましたが、ステップ自体は手動で定義していました。今回はステップ自体も繰り返し実行する方法をご紹介したいと思います。


https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml

長いので1stビューに収(ry

1ステップ目までは前回までと同じです。違うのは2ステップ目と3ステップ目で、ステップを分けて繰り返し同じAPIを実行していました。

メインの2ステップ目を見ていきましょう

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L23-L35

こちらの内容で、すべての記事の詳細にアクセス出来るようになります。
今回の変数のcountは3になっているので3件アクセスとなります。
以前の ["リクエストの結果を次のステップで利用してみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day08) ではcountを1件にして絞り込みを行っていました。

詳しく見ていきましょう

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L25

この `loop` セクションで繰り返し実行数を指定します。
`len(articles)` で `articles` の件数になります。
急に出てきた `articles` ですが、一つ前のステップで別名定義しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L21-L22

忘れていた方は ["任意の値に別名をつける"](https://zenn.dev/katzumi/articles/runn-tutorial-day10) を復習すると良いでしょう。

次に重要なのがこちら

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L27

以前は `articles[0].slug` 若しくは `articles[1].slug` と手動で書き換えていました。
`i` はループ変数です。 `loop` セクションがあるステップでは自動で定義されます。
`articles[i].slug` で　`articles[0].slug` ... `articles[len(articles) - 1].slug` まで順次アクセスされるようになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day11/loop.yml#L34-L35

もちろん `test` でもループ変数 `i` は使えます。

如何でしょうか？すごく簡単にループ処理も実現できたかと思います。

一点注意点として繰り返し実行するとDoS攻撃になってしまいます。
開発中のテスト環境のサーバーであれば問題ないですが、外部サービス等へのアクセスは適切にインターバルを挟むようにしましょう。

詳細はオフィシャルのパラメータを参照してください。

https://github.com/k1LoW/runn#interval
https://github.com/k1LoW/runn#retry-runbook

明日は「既存のシナリオから新しいシナリオを作成する」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day12