---
title: "外部コマンドを実行してみる"
free: true
---

# 外部コマンドを実行してみる

[前のチャプタ](https://zenn.dev/katzumi/books/runn-tutorial/viewer/openapi) で OpenAPI 仕様書によるリクエストとレスポンスの検証ができるようになりました。 
今回は更に OpenAPI 仕様書を利用した場合の便利な機能を紹介したいきます。


https://github.com/k2tzumi/runn-tutorial/blob/main/day19/open-api-coverage.yml

前回の runbook からステップ 1 つ追加しました。

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/open-api-coverage.yml#L23-L36

単体テストではカバレッジが計測できますが、API テストではアプリケーションのステップ単位でのカバレッジを計測することが通常出来ません。

今回のお題の外部コマンドを実行して API 仕様書のカバー範囲を確認する方法は以下の通りになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/exec-coverage.yml

外部コマンドを実行するには Exec Runner を利用します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/exec-coverage.yml#L5-L6

Exec Runner は組み込みの Runner で設定不要で利用できます。
`command` セクションで実行するコマンドを指定します。
コマンドの実行結果はそれぞれ以下のように参照できます。

* exit_code  
終了コード。
* stdout  
標準出力。
* stderr  
標準エラー出力。

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/exec-coverage.yml#L10

dump でコマンドの結果を出力しています。
Dump runner の使い方は ["Hello world!"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/hello-world) と ["レスポンスをdumpして外部JSON化する"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/dump) で紹介しています。


Exec Runner を使う際の注意点として、実行時のオプションの追加が必要な点です。

```console
$ runn run --scopes run:exec day19/exec-coverage.yml
  Spec                                     Coverage  
-----------------------------------------------------
  Total                                       10.5%  
    Swagger Petstore - OpenAPI 3.0:1.0.17     10.5%  

.

1 scenario, 0 skipped, 0 failures
```

`--scopes run:exec` オプションになります。
実行結果としてカバレッジが出力されます。

サンプルで Exec Runner を利用していますが、もちろん直接以下のように実行してもカバレッジ表示ができます。

```console
$ runn coverage day19/open-api-coverage.yml
```

普段は run コマンドを実行していますが、coverage コマンドに変えるだけです。

API のテスト漏れがないか？偶には API のシナリオテストでのカバレッジを見てみると良いかと考えます。

