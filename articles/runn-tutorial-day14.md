---
title: "【Day14】include専用シナリオを活用する / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-14 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/14 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートして 2 週間経ちました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[includeしたシナリオの変数を書き換えよう](https://zenn.dev/katzumi/articles/runn-tutorial-day13)」でした。

## include専用シナリオを活用する

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day13) で include するシナリオに引数を与えることができるようになりました。 
今回は更に便利に引数を引き継ぐ方法を紹介したいきます。

今回は 3 つの runbook を利用します。

* show-article.yml  
前回と同じ。
* include-list-articles.yml  
前回の `list-articles.yml` を改変したもの。
* include-use-parent-vars.yml  
前回の `include-with-vars.yml` を改変したもの。

上から順番に説明していきます。

:::details show-article.yml

前回と同じです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day14/show-article.yml


:::

次に include-list-articles.yml です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day14/include-list-articles.yml

vars セクションの値が `{{ parent.vars.username }}` となっています。
今回の肝はこちらの記述方法になります。

こちらの記述にすると include 元の vars にアクセスすることが出来ます。
柔軟度は高くなりますが、こちらの記述を使うことで include 専用のシナリオ（runbook）になります。
include しないと `parent` にアクセスできない為です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day14/include-list-articles.yml#L8

include 専用の runbook にはこちらの記述が必要になります。
こちらの `if` セクションは、runbook の実行条件を定義できます。
`included` は include された runbook 上では true になります。つまり、include 時のみに実行できます。

実際に実行してみます。

```console
$ runn run day14/include-list-articles.yml --debug --verbose
Skip include専用のシナリオを定義
=== include専用のシナリオを定義 (day14/include-list-articles.yml) ... ok


1 scenario, 1 skipped, 0 failures
```

`1 skipped` になっており、スキップされていることがわかります。

次に include-use-parent-vars.yml を見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day14/include-use-parent-vars.yml

違いは vars セクションの追加のみです、
でもこちらの runbook 内には記述はありませんが、include 時に vars を指定しなくても引き継ぐことが出来ています。
include 時に vars を上書きするには前回の ["includeしたシナリオの変数を書き換えよう"](https://zenn.dev/katzumi/articles/runn-tutorial-day13) を参照ください。

include 専用の runbook を用意することでカスケード的に引数を引き継ぐことができる様になります。

全体実行させてみると以下のような感じになります。

```console
$ runn run day14/**/*.yml --verbose        
=== include専用のシナリオを定義 (day14/include-list-articles.yml) ... skip
    --- 指定された件数分、記事一覧を取得します (listArticles) ... skip
=== include専用のシナリオを定義して親のvarsを引き継がせる (day14/include-use-parent-vars.yml) ... ok
    --- varsで指定された件数分、記事一覧を取得します (listArticles) ... ok
        === include専用のシナリオを定義 (day14/include-list-articles.yml) ... ok
            --- 指定された件数分、記事一覧を取得します (listArticles) ... ok
    --- 1番目の記事の詳細を取得します (showFirstArticle) ... ok
        === 変数を受け取るシナリオを定義 (day14/show-article.yml) ... ok
            --- 記事の詳細を取得します (showArticle) ... ok
    --- 2番目の記事の詳細を取得します (showSecondArticle) ... ok
        === 変数を受け取るシナリオを定義 (day14/show-article.yml) ... ok
            --- 記事の詳細を取得します (showArticle) ... ok
=== 変数を受け取るシナリオを定義 (day14/show-article.yml) ... ok
    --- 記事の詳細を取得します (showArticle) ... ok


3 scenarios, 1 skipped, 0 failures
```

明日は「リクエスト JSON を外部ファイル化する」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day15