---
title: "【Day13】includeしたシナリオの変数を書き換えよう / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-13 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/13 配信になります。
<!-- markdownlint-disable-next-line ja-technical-writing/ja-no-mixed-period -->
:::

## はじめに

一人アドベントカレンダーとしスタートして後半戦に突入しました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[既存のシナリオから新しいシナリオを作成する](https://zenn.dev/katzumi/articles/runn-tutorial-day12)」でした。

## includeしたシナリオの変数を書き換えよう

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day12) で include を使ってシナリオの共通化ができるようになりました。 
今回は更に発展して共通化する範囲を広げる際のテクニックを紹介します。

今回は 3 つの runbook を利用します。

* list-articles.yml  
前回と同じ。
* show-article.yml  
新規。前回の include 元のステップを切り出したもの。
* include-with-vars.yml  
前回のメインと同じものを今回用にブラッシュアップしたもの。

各 runbook の依存関係がわかりやすいように先に実行結果を記述しておきます。

```console
$ USER=katzumi runn run day13/include-with-vars.yml --verbose
=== includeしたシナリオの変数を変更して共通化させよう (day13/include-with-vars.yml) ... ok
    --- 指定された件数分、記事一覧を取得します (listArticles) ... ok
        === 単体のシナリオとして定義 (day13/list-articles.yml) ... ok
            --- 指定された件数分、記事一覧を取得します (listArticles) ... ok
    --- 1番目の記事の詳細を取得します (showFirstArticle) ... ok
        === 変数を受け取るシナリオを定義 (day13/show-article.yml) ... ok
            --- 記事の詳細を取得します (showArticle) ... ok
    --- 2番目の記事の詳細を取得します (showSecondArticle) ... ok
        === 変数を受け取るシナリオを定義 (day13/show-article.yml) ... ok
            --- 記事の詳細を取得します (showArticle) ... ok


1 scenario, 0 skipped, 0 failures
```

上から順番に説明していきます。

:::details list-articles.yml

前回と同じ。

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/list-articles.yml


:::

次に show-article.yml です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/show-article.yml

vars セクションで slug を変数化して実行できるようになっています。
vars の説明は ["変数を使ってみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day05) を参照ください。

もちろんこちらも単体で実行できます。

次にメインの include-with-vars.yml です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/include-with-vars.yml

なんとかギリギリ 1st ビューに収まるぐらいになっていますｗ
繰り返しになりますが、前回とシナリオの内容は同じです。
2,3 ステップ目も include を使うようになり、大分スッキリしたのではないでしょうか？

詳細を見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/include-with-vars.yml#L8-L13

include の書き方が少し変わっていますが `show-article.yml` を include しているのがわかります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/include-with-vars.yml#L12-L13

include セクションの中に vars の宣言があります。

こちらの記述で `show-article.yml` で定義されている `vars.slug` が置き換えて実行されます。

敢えて実行結果を載せませんが、前回の実行結果と同じになりますので試してみてください。

```console
$ USER=katzumi runn run day13/include-with-vars.yml --debug
```

include と vars の組み合わせで 1 つの runbook がモジュールの様に扱えるのが理解頂けたかと考えます。　
単体の runbook なら `vars` と `bind` はそれほど使わなくてもいいと考えていますが、共通化する runbook は必要に応じてそれぞれ変数を定義して使いやすくしていきましょう！

明日は「include 専用シナリオを活用する」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day14