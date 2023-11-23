---
title: "【Day7】レスポンスの結果を検証してみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-07 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/07 配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして 1 週間経ちました。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日全部理解したら一人で runn を使って API シナリオテストや、ちょっとした API と連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は「[変数を外部から与えててみる](https://zenn.dev/katzumi/articles/runn-tutorial-day06)」でした。

## レスポンスの結果を検証してみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day06) で環境変数を与えて実行するシナリオ内容の動きを変える方法を説明しました。 
シナリオの動きを変えていくと、その動きに応じてレスポンスが変わったということを実践できたと思います。

今回はこのレスポンスが期待通りに変わっているのか？を検証する方法を説明したいと思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day07/test.yml

今回で GET パラメータをすべて変数化がされました。

今回新しく増えたセクションは `test` になります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day07/test.yml#L15-L19

2 つチェック観点として記述しています。
まずは1つ目はステータスコードが 200 になっていることです。
同一ステップ内のステータスコードを参照するには `current.res.status` となります。
レスポンス Body の参照は `current.res.body` になります。
2 つ目のチェックはレスポンス Body の JSON の検証を行っています。
`len(current.res.body.articles)` でレスポンス JSON 内の `articles` の要素数をカウントしています。
それと `vars.count` で変数のカウントと同じであることを確認しています。
変数の使い方は以下の記事を参照してください。

https://zenn.dev/katzumi/articles/runn-tutorial-day05

`vars.count` はリクエストの GET パラメータで指定しており、数を変更すると記事のレスポンス件数が変わります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day07/test.yml#L9-L11

実際に以下の行の数値を変更して挙動が変わるか？（またテストとしては OK になるか？）動きを試してみてください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day07/test.yml#L6

例えば 1000 件として実行してみると以下の様なエラーメッセージが表示されると思います。

```console
$ USER=katzumi runn run day07/test.yml        
.

1) day07/test.yml 2320a0907acd9ada78c7a41aa0dd2b7639a0b53e
  Failure/Error: test failed on "レスポンスの内容をチェックしてテスト品質を高めましょう".steps[0] "指定された件数分、記事一覧を取得します": condition is not true
  
  Condition:
    current.res.status == 200
    && len(current.res.body.articles) == vars.count
    │
    ├── current.res.status => 200
    ├── 200 => 200
    ├── len(current.res.body.articles) => 22
    ├── current.res.body.articles => [{ .. snip ..}}]
    └── vars.count => 1000
    
  Failure step (day07/test.yml):
   9 - desc: "指定された件数分、記事一覧を取得します"
  10   req:
  11     /api/articles?username={{ vars.username }}&count={{ vars.count }}&order={{ vars.order }}:
  12       get:
  13         body:
  14           application/json: null
  15   test: |
  16     # ステータスコードが200であること
  17     current.res.status == 200
  18     # 記事の件数が正しいこと
  19     && len(current.res.body.articles) == vars.count


1 scenario, 0 skipped, 1 failure
```

エラーの見方ですが、`Condition` の部分で test セクションの結果、`Failure step` でエラーが発生したステップが表示されます。

Condition を詳しく見ていくと

`current.res.status => 200`

でステータスコードは正しく

`vars.count => 1000` 

 で 1000 件まで指定したけれど

`len(current.res.body.articles) => 22` 

実際のレスポンス JSON 内の記事が 22 件しか存在しなかったのでエラーになったことがわかります。

明日は「リクエストの結果を次のステップで利用してみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day08