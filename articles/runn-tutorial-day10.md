---
title: "【Day10】任意の値に別名をつける / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー","runn"]
published: true
published_at: 2023-12-10 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/10 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートして二桁日目に突入しました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[ステップに名前を付ける](https://zenn.dev/katzumi/articles/runn-tutorial-day09)」でした。

## 任意の値に別名をつける

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day09) で 3 つ以上のステップも扱えるようになりました。 
複数のステップの色々な値を組み合わせながらステップを組み立てていくことが出来る様になりました。

今回はその色々な値をもっと便利に参照できる方法を紹介します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day10/bind.yml

長いので 1st ビューに収ま【略】。

今回は前回のシナリオと内容はまったく同じです。書き方だけ変えています。

ステップごとに見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day10/bind.yml#L9-L22

違いは最後の 2 行です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day10/bind.yml#L21-L22

ここのみです。
`bind` セクションでは色々な値を別名で参照できる様にするものです。
繰り返し参照するものについて使うと大変便利です。

2 ステップ目です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day10/bind.yml#L23-L34

前回との違いはこちら。

https://github.com/k2tzumi/runn-tutorial/blob/main/day10/bind.yml#L26

`steps.listArticles.res.body.articles[0].slug` が `articles[0].slug` と非常に短く書けます。
`articles[0].id` も短くなっています。

3 ステップ目も同様。

https://github.com/k2tzumi/runn-tutorial/blob/main/day10/bind.yml#L35-L46

bind は複数ステップで同じ値を使う場合にその効果が発揮できます。

明日は「繰り返し処理を行ってみよう」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day11