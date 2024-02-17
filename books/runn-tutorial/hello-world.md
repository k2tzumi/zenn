---
title: "Hello world! "
free: true
---

# Hello world！

runn のシナリオは runbook という yaml 形式で定義します。

runn からコンソールに文字を出力する方法は幾つかありますが、一番オーソドックスなもので実装します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day02/hello_world.yml

runn の実行コマンドは `runn run runbookのパス` の形式となります。
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

上記のハイフンから dump の部分がシナリオのステップの中身になり、こちらが 1 つのステップとなります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day02/hello_world.yml#L4

`dump` は、Dump Runner と呼ばれるもので自動組み込みされる Runner で、何も定義しなくても使えます。  
こちらの指定された値を表示します。
runbook が yaml で書く関係上、数値と文字列は区別して書く必要があり、`"'Hello world!'"` といった形でダブルコーテーションとシングルコーテーションを利用して文字列を表現しています。