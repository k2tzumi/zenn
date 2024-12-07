---
title: "GitHub Actionsで連続pushした時に止めるアレ"
emoji: "⛔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["githubactions","ci"]
published: true
---


大分時間が経ってしまいましたが、2022/8/31 に開催された stand.fm 主催の [TECH STAND #9 GitHub](https://standfm.connpass.com/event/256786/) イベントに参加しました。  

その際に呟いたやつが今回の記事の内容です  
https://twitter.com/katzchum/status/1564933547751665665

有り難いことに直ぐにフォロー頂きました。

https://twitter.com/Shitimi_613/status/1564934755271196672

あまり纏まった記事が見当たらなかったので、自分用のメモとしてまとめます。

## 他のCIにはあったアレ

GitHub Actions を利用する前は、TravisCI や CircleCI を利用していました。  
移行してから随分使ってないので、記憶が定かではないのですが

```
連続してpushすると一つ前の実行中のjobが停止される
```

という機能が標準であった気がします。  
この機能の名前は何と呼ぶのでしょうか？地味だけれども、ないと困るアレですｗ

GitHub Actions のリリース直後にこちらの機能と `[ci skip]` が使えずに後発なサービスなのにーと不満を覚えていました。  

その後にアレの機能を実装したカスタムアクションを組み込んで対応いました。  
以下の様な action を実際に組み込まれた方は多かったのではないかと思います。

https://github.com/marketplace/actions/workflow-run-cleanup-action
https://github.com/marketplace/actions/skip-duplicate-actions

## アレ = concurrency

オフィシャルの日本語ドキュメントはこちら  
https://docs.github.com/ja/actions/using-jobs/using-concurrency

これまたありがたいことに、イベントでフォロー頂いた [@korosuke613](https://zenn.dev/korosuke613) さんが挙動をスクラップにまとめてくれていました。  
勝手ながら紹介させて頂きます。  
https://zenn.dev/korosuke613/scraps/4e465aad5538d0

## 一般的なconcurrencyの設定の仕方

`concurrency` 自体は、グループ名を細かく指定すれば複雑な workflow であっても止める（待つ）所とそうでない所を色々制御できそうです。  
ただ workflow の実行頻度が一番高いテストは単純なものが多く、逆にグループ名の付け方を悩む人 ^[自由度が高いとあれこれ考えてしまう自分みたいな人] もいると思います。 
個人的にベストだと考えている設定内容はこちらです。  

```yaml
name: test
on:
  pull_request:
    types: [opened, reopened, synchronize]
    paths:
      - 'foo/**'
  push:
    branches:
      - develop

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

よくある PR 時と develop に merge した時に動く workflow の設定です。  
これで 1 つのブランチで 1 つのワークフローが実行されるようになると思います。  
workflow 毎に設定変更しなくて済み、とりあえず入れておくかーという感じで使えています。  

注意点として別の workflow ファイルでも `name` が同じだと、同じ workflow だと判断されて止まってしまいます。  
例えば単体テストと E2E テストで別 workflow にした場合には、別の名前にしましょう。

## 最後に

自分の様に古いカスタムアクションを利用して workflow をキャンセルしている人は移行をおすすめします。  
キャンセルするカスタムアクション自体が1つの workflow として動くので、リソースは無駄ですし、待ち時間も微妙に伸びます。  
やはり標準機能を使うのが正義かと。  
concurrency についてもっと良い書き方や、こういうユースケースでも使っています！というのがあれば、コメントを頂けると助かります。  