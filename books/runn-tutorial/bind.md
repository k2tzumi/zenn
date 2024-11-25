---
title: "任意の値に別名をつける"
free: true
---

# 任意の値に別名をつける

[前のチャプタ](https://zenn.dev/katzumi/books/runn-tutorial/viewer/ordered-map) で 3 つ以上のステップも扱えるようになりました。 
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
