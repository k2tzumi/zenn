---
title: "【Day20】DBに結果を格納する / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-20 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/20配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートしてそろそろラストスパートです。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[外部コマンドを実行してみる](https://zenn.dev/katzumi/articles/runn-tutorial-day19)」でした。

## DBに結果を格納する

runnが対応しているデータベースは幾つかありますが、今回はSQLite3を使ってみます。

前準備でスキーマ作成から行います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml

まずrunnersセクションから見ていきます

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml#L2-L3

`sq://` スキーマでSQLiteのファイルパスを指定します。
`local`という名前でDB Runnerを定義しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml#L7-L26

`query` セクションで実行するクエリを指定しています。
こちらのクエリでテーブル作成しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/init-db.yml#L27-L34

次のステップで作成したテーブルの確認をしています。
`if: !included` でこちらのステップはinclude *されなかった* 場合にのみ実行されます。

`select * from sqlite_master;` でテーブル一覧を取得しています。
testでは `one(current.rows, {.tbl_name == 'article'})` をチェックしています。
コメントに記載していますが、テーブル一覧にarticleが含まれていることを確認しています。　　
runnは式評価に [antonmedv/expr](https://github.com/antonmedv/expr) を利用しています。
こちらの式評価で使える記法については [こちら](https://github.com/antonmedv/expr/blob/master/docs/Language-Definition.md) を参照してください。
うまく使うとテスト内容を柔軟に定義することが出来るようになります。

データベースの準備ができましたので本題のシナリオに入ります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml

長いのでステップごとにみています

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L9-L11

こちらで先程のDB初期化がされます。
includeの使い方は ["既存のシナリオから新しいシナリオを作成する"](https://zenn.dev/katzumi/articles/runn-tutorial-day12) を参照してください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L12-L23

こちらのステップで記事一覧を取得します。
こちらのステップの内容については ["任意の値に別名をつける"](https://zenn.dev/katzumi/articles/runn-tutorial-day10)　にまとめていますので、ご参考までに。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L24-L43

こちらが本記事のメインの処理となります。
ループしながら、insert文を繰り返し実行しています。
loopの使い方は ["繰り返し処理を行ってみよう"](https://zenn.dev/katzumi/articles/runn-tutorial-day11) を参照ください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L39-L43

テストの内容ですが

* last_insert_id  
最後にinsertされたレコードのID
* rows_affected  
クエリ実行後の影響のあった行数。今回のINSERT文では1レコードづつ入れているので、1になります

となります。

いよいよ最後のステップです

https://github.com/k2tzumi/runn-tutorial/blob/main/day20/use-db.yml#L44-L48

`select sum(liked_count) as sum_likes from article;`

こちらのクエリでいいね数を集計しています。
`dump` で取得した内容を出力しています。
こちらのクエリでは1行しかレコードが無いので `current.rows[0].sum_likes` という感じで集計結果が参照できます。

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

いいね数のTOTALが516なのがわかります。
`--debug` オプションの方がより動きがわかりやすいと思うので、ぜひ実行してみてください。

如何でしたでしょうか？データベースを利用することで、複雑なシナリオを実現できるようになります。
APIシナリオテストですが、全てのデータに対してAPIが提供されていなかったりする場合にこちらのデータベースへのアクセスは非常に有効になります。
API呼び出し後に、正しくデータベースに格納されたか？ということもこちらを使えば実現できるようになります。

明日は「CDPを使ってブラウザ操作してみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day21