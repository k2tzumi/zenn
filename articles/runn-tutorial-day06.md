---
title: "【Day6】変数を外部から与えてみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","runn","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-06 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/06 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[変数を使ってみる](https://zenn.dev/katzumi/articles/runn-tutorial-day05)」でした。

## 変数を外部から与えてみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day05) で変数の定義と参照する方法を説明しました。 
今回は変数を外部から与える方法について説明します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day06/env.yml

変数を増やしていたりもしますが、増えた記述方法は 1 つのみです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day06/env.yml#L5

`${USER:-katzumi}` の箇所です。
Docker compose の yaml を記述した人ならお馴染みの環境変数展開が可能になっています。

こちらの定義で、`USER` という環境変数を参照しています。
また `${変数:-default}` という記法を行うとデフォルト値が適用されます。  
runn に環境変数を与えながら実行するには以下のようにします。

```console
$ USER=zenn runn run day06/env.yml --debug
```

なお一部 OS やシェルではデフォルトで `USER` が定義されていますので未指定の場合に 404 エラーが発生するかもしれませんのでご注意ください。

こちらの環境変数の展開を利用すると、環境依存の記述を排除できます。

* 1 つの runbook で本番環境とテスト環境の実行を切替る
* 環境変数によって動作を変更させることができる

といったことが出来るようになりますので、意識的に環境変数を埋め込んでいくことをオススメします。

明日は「レスポンスの結果を検証してみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day07