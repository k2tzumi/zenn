---
title: "【Day9】ステップに名前を付ける / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-09 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/09 配信になります。
<!-- markdownlint-disable-next-line ja-technical-writing/ja-no-mixed-period -->
:::

## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[リクエストの結果を次のステップで利用してみる](https://zenn.dev/katzumi/articles/runn-tutorial-day08)」でした。

## ステップに名前を付ける

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day08) で 2 つのステップを定義して API を組み合わせたテストを行えるようになりました。 

今回はもっと複雑に 3 ステップ以上のシナリオを書いていきます。

### listのままの場合

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/lists.yml

長いので 1st ビューに収まらないです、。

1 ステップ目はまったく同じです。
2 ステップ目から見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/lists.yml#L20-L30

やっている内容は前回とまったく同じですが、前回は `current` と `previous` を参照していた書き方が変わりました。

|内容|変更前|変更後|
|---|---|---|
|一つ前のステップの1番目の記事のslug|`previous.res.body.articles[0].slug`|`steps[0].res.body.articles[0].slug`|
|現在のステップの記事のID|`current.res.body.article.id`|`steps[1].res.body.article.id`|
|一つ前のステップの1番目の記事のID|`previous.res.body.articles[0].id`|`steps[0].res.body.articles[0].id`|

それぞれ変わっています。
上記部分ではそれぞれ以下のように対応します。

* `current` が `steps[1]`
* `previous` が `steps[0]`

`steps[ステップ番号]` で参照できるのがわかります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/lists.yml#L31-L41

3 ステップ目はこちらになります。
`steps[2]` という変数が現れましたが、3 ステップ目を表しているのがわかります。
`steps[2]` から参照すると 1 つ前（`previous`）が `steps[1]` になります。
実は `current` と `previous` はこの `steps[ステップ番号]` の alias(別名)になっています。

前回までは 2 ステップしかなかったので、問題なかったのですが 3 ステップ目になると単純に 1 つ前だけでなく 2 つ、3 つ前も参照したくなります。
その場合にステップ番号でのアクセスする方法では大変です。ステップ番号を覚えるのも大変ですし、途中でステップの追加も発生する可能性があります。

ということで本題にあるステップの名前をつけたくなります。

### mapにする

新しい記述方法はこちらです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml

yaml の steps セクションの書き方が list から map に変わっているのがわかります。

1 つ目のステップを見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml#L9-L20

`listArticles` というステップ名にしています。

次に 2 つ目のステップはこちら。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml#L21-L32

`showFirstArticle` というステップ名にしています。

|内容|変更前|変更後|
|---|---|---|
|1番目のステップの1番目の記事のslug|`steps[0].res.body.articles[0].slug`|`steps.listArticles.res.body.articles[0].slug`|
|2番目のステップの記事のID|`steps[1].res.body.article.id`|`steps.showFirstArticle.res.body.article.id`|
|1番目のステップの1番目の記事のID|`steps[0].res.body.articles[0].id`|`steps.listArticles.res.body.articles[0].id`|

`steps[ステップ番号]` という記述よりもだいぶ可読性が上がったのでは無いでしょうか？
雑に runbook を作成する場合は今まで通り list 形式で作成するのが早いですが、メンテナンスし続けるシナリオであれば map 形式 1 択です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day09/maps.yml#L33-L44

3 ステップ目です。
こちらはもう既に説明不要かと考えます。
説明の為に冗長に記載していますが、`steps.showSecondArticle.res.body.article.id` の部分は今まで通り `current.res.body.article.id` としたほうがわかりやすいです。

map 形式にすれば `previous` をほぼ使うことはありませんが、`current` はステップ名を変更した場合も影響を受けなくすることが出来るのでオススメです。

最後に runn 実行時のオススメのオプションを紹介します。`--verbose` オプションです。

* list 形式の場合  
    ```console
    $ USER=katzumi runn run day09/lists.yml --verbose
    === ステップに名前をつけてフローを追いやすくしましょう(listのままの場合) (day09/lists.yml) ... ok
        --- 指定された件数分、記事一覧を取得します (0) ... ok
        --- 1番目の記事の詳細を取得します (1) ... ok
        --- 2番目の記事の詳細を取得します (2) ... ok


    1 scenario, 0 skipped, 0 failures
    ```
* map 形式の場合
    ```console
    $ USER=katzumi runn run day09/maps.yml --verbose 
    === ステップに名前をつけてフローを追いやすくしましょう (day09/maps.yml) ... ok
        --- 指定された件数分、記事一覧を取得します (listArticles) ... ok
        --- 1番目の記事の詳細を取得します (showFirstArticle) ... ok
        --- 2番目の記事の詳細を取得します (showSecondArticle) ... ok


    1 scenario, 0 skipped, 0 failures
    ```

この様にステップの詳細及び結果を表示できます。
比較すると map 形式の方がわかりやすいです！

明日は「任意の値に別名をつける」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day10