---
title: "【Day13】includeしたシナリオの変数を書き換えよう / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-13 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/13配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして後半戦に突入しました。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[既存のシナリオから新しいシナリオを作成する](https://zenn.dev/katzumi/articles/runn-tutorial-day12)」でした。

## includeしたシナリオの変数を書き換えよう

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day12) でincludeを使ってシナリオの共通化ができるようになりました。 
今回は更に発展して共通化する範囲を広げる際のテクニックを紹介したいと思います。

今回は3つのrunbookを利用します。

* list-articles.yml  
前回と同じ
* show-article.yml  
新規。前回のinclude元のステップを切り出したもの
* include-with-vars.yml  
前回のメインと同じものを今回用にブラッシュアップしたもの

上から順番に説明していきます。

:::details list-articles.ymlは前回と同じ

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/list-articles.yml

:::

次にshow-article.ymlです

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/show-article.yml

varsセクションでslugを変数化して実行できるようになっています。
varsの説明は ["変数を使ってみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day05) を参照

もちろんこちらも単体で実行できます。

次にメインのinclude-with-vars.ymlです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/include-with-vars.yml

なんとかギリギリ1stビューに収まるぐらいになっていますｗ
繰り返しになりますが、前回とシナリオの内容は同じです。
2,3ステップ目もincludeを使うようになり、大分スッキリしたのではないでしょうか？

詳細を見ていきましょう

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/include-with-vars.yml#L8-L13

includeの書き方が少し変わっていますが `show-article.yml` をincludeしているのがわかると思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day13/include-with-vars.yml#L12-L13

includeセクションの中にvarsの宣言があります。

こちらの記述で `show-article.yml` で定義されている `vars.slug` が置き換えて実行されます。

敢えて実行結果を載せませんが、前回の実行結果と同じになります。試してみてください

```console
$ USER=katzumi runn run day13/include-with-vars.yml --debug
```

includeとvarsの組み合わせで一つのrunbookがモジュールの様に扱えるのが理解頂けたかと思います。　　
単体のrunbookなら `vars` と `bind` はそれほど使わなくてもいいと思いますが、共通化するrunbookは必要に応じてそれぞれ変数を定義して使いやすくしていきましょう！

明日は「include専用シナリオを活用する」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day14