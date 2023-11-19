---
title: "【Day6】変数を外部から与えててみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-06 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/06配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみてもし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[変数を使ってみる](https://zenn.dev/katzumi/articles/runn-tutorial-day05)」でした。

## 変数を外部から与えててみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day05) で変数の定義と参照する方法を説明しました。 
今回は変数を外部から与える方法について説明していきたいと思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day06/env.yml

変数を増やしていたりもしますが、増えた記述方法は一つのみです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day06/env.yml#L5

`${USER:-katzumi}` の箇所です。
Docker composeのyamlを記述した人ならお馴染みの環境変数展開が可能になっています。

こちらの定義で、`USER` という環境変数を参照しています。
また `${変数:-default}` という記法を行うとデフォルト値が適用されます。  
runnに環境変数を与えながら実行するには以下のようにします。

```console
$ USER=zenn runn run day06/env.yml --debug
```

なお一部OSやシェルではデフォルトで `USER` が定義されていますので未指定の場合に404エラーが発生するかもしれませんのでご注意を

こちらの環境変数の展開を利用すると、環境依存の記述が変数化ができ

* 一つのrunbookで本番環境とテスト環境の実行が切り替えれる
* 環境変数によって動作を変更させることができる

といったことが出来るようになりますので、意識的に環境変数を埋め込んでいくことをオススメします。

明日は「レスポンスの結果を検証してみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day07