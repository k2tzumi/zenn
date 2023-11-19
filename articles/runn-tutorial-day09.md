---
title: "【Day9】ステップに名前を付ける / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-09 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/09配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[リクエストの結果を次のステップで利用してみる](https://zenn.dev/katzumi/articles/runn-tutorial-day08)」でした。

## ステップに名前を付ける

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day08) で2つのステップを定義してAPIを組み合わせたテストを行えるようになりました。 

今回はもっと複雑に3ステップ以上のシナリオを書いていきたいと思います。

### listのままの場合

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/lists.yml

長いので1stビューに収まらない。。

1ステップ目はまったく同じです。
2ステップ目から見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/lists.yml#L20-L30

やっている内容は前回とまったく同じですが、前回は `current` と `previous` を参照していましたが、書き方が変わりました。

|内容|変更前|変更後|
|---|---|---|
|一つ前のステップの1番目の記事のslug|`previous.res.body.articles[0].slug`|`steps[0].res.body.articles[0].slug`|
|現在のステップの記事のID|`current.res.body.article.id`|`steps[1].res.body.article.id`|
|一つ前のステップの1番目の記事のID|`previous.res.body.articles[0].id`|`steps[0].res.body.articles[0].id`|

それぞれ変わっています。
上記部分では

* `current` が `steps[1]`
* `previous` が `steps[0]`

に対応することがわかると思います。
`steps[ステップ番号]` で参照できるのがわかると思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/lists.yml#L31-L41

3ステップ目はこちらになります。
`steps[2]` という変数が現れましたが、3ステップ目を表しているのがわかると思います。
`steps[2]` から参照すると一つ前（ `previous` ）が `steps[1]` になります。
実は`current`と`previous`はこの `steps[ステップ番号]` のalias(別名)になっています。

前回までは2ステップしかなかったので、問題なかったのですが3ステップ目になると単純に一つ前だけでなく2つ、3つ前も参照したくなります。
その場合にステップ番号でのアクセスする方法では大変です。ステップ番号を覚えるのも大変ですし、途中でステップの追加も発生する可能性もあります。

ということで本題にあるステップの名前をつけたくなります。

### mapにする

新しい記述方法はこちらです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml

yamlのstepsセクションの書き方がlistからmapに変わっているのがわかると思います。

1つ目のステップ

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml#L9-L20

`listArticles` というステップ名にしています。

2つ目のステップ

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml#L21-L32

`showFirstArticle` というステップ名にしています。

|内容|変更前|変更後|
|---|---|---|
|1番目のステップの1番目の記事のslug|`steps[0].res.body.articles[0].slug`|`steps.listArticles.res.body.articles[0].slug`|
|2番目のステップの記事のID|`steps[1].res.body.article.id`|`steps.showFirstArticle.res.body.article.id`|
|1番目のステップの1番目の記事のID|`steps[0].res.body.articles[0].id`|`steps.listArticles.res.body.articles[0].id`|

`steps[ステップ番号]` という記述よりもだいぶ可読性が上がったのでは無いでしょうか？
雑にrunbookを作成する場合は今まで通りlist形式で作成するのが早いですが、メンテナンスし続けるシナリオであればmap形式1択です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml#L33-L44

3ステップ目です。
こちらはもう既に説明不要かと思います。
説明の為に冗長に記載していますが、

`steps.showSecondArticle.res.body.article.id` の部分は今まで通り `current.res.body.article.id` としたほうがわかりやすいです。

map形式にすれば `previous` はほぼ使うことはありませんが、`current` はステップ名を変更した場合も影響を受けなくすることが出来るのでオススメです。


明日は「任意の値に別名をつける」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day10