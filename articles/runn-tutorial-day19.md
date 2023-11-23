---
title: "【Day19】外部コマンドを実行してみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-19 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/19配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして絶賛掲載中です。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[OpenAPIの仕様書通りか？テストする](https://zenn.dev/katzumi/articles/runn-tutorial-day18)」でした。

## 外部コマンドを実行してみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day18) でOpenAPI仕様書によるリクエストとレスポンスの検証ができるようになりました。 
今回は更にOpenAPI仕様書を利用した場合の便利な機能を紹介したいと思います。


https://github.com/k2tzumi/runn-tutorial/blob/main/day19/open-api-coverage.yml

前回のrunbookからステップ1つ追加しました。

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/open-api-coverage.yml#L23-L36

単体テストを行う際にカバレッジを計測できるのですが、APIテストではソース行でのカバレッジを計測することは通常は出来ません。

今回のお題の外部コマンドを実行してAPI仕様書のカバー範囲を確認する方法は以下の通りになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/exec-coverage.yml

外部コマンドを実行するには Exec Runner を利用します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/exec-coverage.yml#L5-L6

Exec Runnerは組み込みのRunnerで設定不要で利用できます。
`command` セクションで実行するコマンドを指定します。
コマンドの実行結果はそれぞれ以下のように参照できます。

* exit_code  
終了コード
* stdout  
標準出力
* stderr  
標準エラー出力

https://github.com/k2tzumi/runn-tutorial/blob/main/day19/exec-coverage.yml#L10

dumpでコマンドの結果を出力しています。
Dump runnerの使い方は ["Hello world!"](https://zenn.dev/katzumi/articles/runn-tutorial-day02) と ["レスポンスをdumpして外部JSON化する"](https://zenn.dev/katzumi/articles/runn-tutorial-day17) で紹介しています。


Exec Runnerを使う際の注意点として、実行時のオプションの追加が必要な点です。

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

サンプルでExec Runnerを利用していますが、もちろん直接

```console
$ runn coverage day19/open-api-coverage.yml
```

でもカバレッジ表示ができます。
普段はrunコマンドを実行していますが、coverageコマンドに変えるだけです。

APIのテスト漏れがないか？偶にはAPIのシナリオテストでのカバレッジを見てみると良いかと思います。

明日は「DBに結果を格納する」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day20