---
title: "【Day20】DBに結果を格納する / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-20 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/20 配信になります。
<!-- markdownlint-disable-next-line ja-technical-writing/ja-no-mixed-period -->
:::

## はじめに

一人アドベントカレンダーとしスタートしてそろそろラストスパートです。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[外部コマンドを実行してみる](https://zenn.dev/katzumi/articles/runn-tutorial-day19)」でした。

## DBに結果を格納する

runn が対応しているデータベースは幾つかありますが、今回は SQLite3 を使ってみます。

前準備でスキーマ作成から行います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml

まず runners セクションから見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml#L2-L3

`sq://` スキーマで SQLite のファイルパスを指定します。
`local` という名前で DB Runner を定義しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml#L7-L26

`query` セクションで実行するクエリを指定しています。
こちらのクエリでテーブル作成しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml#L27-L34

次のステップで作成したテーブルの確認をしています。
`if: !included` でこちらのステップは include *されなかった* 場合にのみ実行されます。

`select * from sqlite_master;` でテーブル一覧を取得しています。
test では `one(current.rows, {.tbl_name == 'article'})` をチェックしています。
コメントに記載していますが、テーブル一覧に article が含まれていることを確認しています。　
runn は式評価に [antonmedv/expr](https://github.com/antonmedv/expr) を利用しています。
こちらの式評価で使える記法については [こちら](https://github.com/antonmedv/expr/blob/master/docs/Language-Definition.md) を参照してください。
うまく使うとテスト内容を柔軟に定義することが出来るようになります。

データベースの準備ができましたので本題のシナリオに入ります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml

長いのでステップごとにみています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L9-L11

こちらで先程の DB 初期化がされます。
include の使い方は ["既存のシナリオから新しいシナリオを作成する"](https://zenn.dev/katzumi/articles/runn-tutorial-day12) を参照してください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L12-L23

こちらのステップで記事一覧を取得します。
こちらのステップの内容については ["任意の値に別名をつける"](https://zenn.dev/katzumi/articles/runn-tutorial-day10)　にまとめていますので、ご参考までに。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L24-L43

こちらが本記事のメインの処理となります。
ループしながら、insert 文を繰り返し実行しています。
loop の使い方は ["繰り返し処理を行ってみよう"](https://zenn.dev/katzumi/articles/runn-tutorial-day11) を参照ください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L39-L43

テストの内容ですが、以下になります。

* last_insert_id  
最後に insert されたレコードの ID。
* rows_affected  
クエリ実行後の影響のあった行数。今回の INSERT 文では 1 レコードづつ入れているので、1 になります。

いよいよ最後のステップです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L44-L48

`select sum(liked_count) as sum_likes from article;`

こちらのクエリでいいね数を集計しています。
`dump` で取得した内容を出力しています。
こちらのクエリでは 1 行しかレコードが無いので `current.rows[0].sum_likes` という感じで集計結果が参照できます。

最後に実行してみます。

```console
$ USER=katzumi runn run day20/use-db.yml --verbose
516
=== DBに結果を格納する (day20/use-db.yml) ... ok
    --- スキーマ作成 (initSchame) ... ok
        === スキーマを定義する (day20/init-db.yml) ... ok
            --- テーブル作成 (crateTable) ... ok
            --- テーブル一覧を確認する (showTables) ... ok
    --- 指定された件数分、記事一覧を取得します (listArticles) ... ok
    --- 記事をテーブルに保存する (saveArticles) ... ok
    --- いいね数をカウントする (countLiked) ... ok


1 scenario, 0 skipped, 0 failures
```

いいね数の TOTAL が 516 なのがわかります。
`--debug` オプションの方が、動きがよりわかりやすいので、ぜひ実行してみてください。

如何でしたでしょうか？データベースを利用することで、複雑なシナリオを実現できるようになります。
API シナリオテストですが、全てのデータに対して API が提供されていなかったりする場合にこちらのデータベースへのアクセスは非常に有効になります。
API 呼び出し後に、正しくデータベースに格納されたか？ということもこちらを使えば実現できるようになります。

明日は「CDP を使ってブラウザ操作してみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day21