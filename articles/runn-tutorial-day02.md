---
title: "【Day2】Hello world! / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-02 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/02配信になります。
:::


## はじめに

一人アドベントカレンダーとしスタートしていました。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[runnをインストールしてみよう](https://zenn.dev/katzumi/articles/runn-tutorial-day01)」でした。

## Hello world!

runnのシナリオはrunbookというyaml形式で定義します。

runnからコンソールに文字を出力する方法は幾つかありますが、一番オーソドックスなもので実装します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day02/hello_world.yml

runnの実行コマンドは `runn run runbookのパス` の形式となります。
実行すると以下のようになります。

```console
$ runn run day02/hello_world.yml
Hello world!
.

1 scenario, 0 skipped, 0 failures
```

最終行が緑色になり、`0 failures` となっていればすべてのシナリオが成功したことを表しています。

各ステップについて説明をしていきます。


https://github.com/k2tzumi/runn-tutorial/blob/main/day02/hello_world.yml#L1

`desc` はシナリオの内容の説明文を入れておくと良いでしょう。なくても動きます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day02/hello_world.yml#L2-L4

`steps` はシナリオのステップを複数定義していきます。こちらのセクションは必ず必要になります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day02/hello_world.yml#L3-L4

上記のハイフンからdumpの部分がシナリオのステップの中身になり、こちらが一つのステップとなります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day02/hello_world.yml#L4

`dump` は、dump runnerと呼ばれるもので自動で組み込みされるrunnerで、何も定義しなくても使えます。  
こちらの指定された値を表示します。
runbookがyamlで書く関係上、数値と文字列は区別して書く必要があり、`"'Hello world!'"` といった形でダブルコーテーションとシングルコーテーションを利用して文字列を表現しています。

明日は「curlコマンドをreplayさせる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day03