---
title: "定点的にカバレッジを取得する"
free: true
---

# 定点的にカバレッジを取得する

[外部コマンドを実行してみる](https://zenn.dev/katzumi/books/runn-tutorial/viewer/exec) では OpenAPI 仕様書に対してのカバレッジを取得するコマンドを紹介しました。  
今回はカバレッジを定点的にウォッチさせる方法を紹介します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day26/open-api.yml

CI で動かす為に、エンドポイントを環境変数から指定できるようにしています。  
また、こちらのシナリオにつなげてテスト対象のエンドポイントを増やします。  

https://github.com/k2tzumi/runn-tutorial/blob/main/day26/coverage-up.yml

findPetById と updatePetNameById のステップが追加されています。

## 実行結果

[こちら](https://github.com/k2tzumi/runn-tutorial/pull/5#issuecomment-1950217700) で確認できます。

![カバレッジコメント](/images/books/runn-tutorial/coverage-report.png)

Pull Request でテスト対象の API の増加や、カバーされたエンドポイントの数で率が変動します。

## 前提事項

カバレッジを定点的にウォッチさせるのに、Github Actions でワークフローを定義して CI 実行させます。  
カバレッジを Pull Request のコメントするカスタムアクションとして [octocov](https://github.com/k1LoW/octocov) を利用します。　　
octocov は各種カバレッジ形式(Go coverage や Clover)にサポートしていますが、runn の OpenAPI 仕様書に対してのカバレッジ形式は標準ではサポートしていません。  
octocov の [カスタムメトリクス](https://github.com/k1LoW/octocov?tab=readme-ov-file#custom-metrics) を利用してレポートさせます。

詳しくは以下の記事を参考にしてください

https://k1low.hatenablog.com/entry/2023/09/06/083000

カバレッジ自体は、runbook に対して静的解析してレポートされますのでカバレッジを取得するだけであればテスト対象の API サーバーは不要です。

## octocovの設定

カスタムメトリクスをレポートさせるにはデフォルトブランチ用とフィーチャーブランチ用の設定ファイルをそれぞれ定義しておきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/.octocov.runn.yml

https://github.com/k2tzumi/runn-tutorial/blob/main/.octocov.runn.main.yml

## Github Actions workflow定義

以下は、API テストを CI 上で実行しつつ、API 仕様書に対してのカバレッジのレポートを行うワークフローです

https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/coverage.yml

詳しく解説していきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/coverage.yml#L17-L25

今回テスト対象の API のサービスをコンテナ実行するようにしています。  
CI を安定化させる為に `health-cmd` を指定して web サーバーの起動チェックを行うようにしています。


https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/coverage.yml#L26-L36

カバレッジ取得せずに API シナリオテストの実行のみであれば、こちらの行のみで OK です。  
CI 実行時にコンテナ起動された API サーバーを参照するように環境変数(`PETSTORE3_END_POINT`) を指定しているのが味噌です。

https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/coverage.yml#L37-L59

main ブランチに対してのカバレッジを取得するステップです。  
main ブランチをチェックアウトしてカバレッジを取得して、main ブランチ用の octocov 設定を利用してレポートしています。


https://github.com/k2tzumi/runn-tutorial/blob/main/.github/workflows/coverage.yml#L60-L68

こちらが Pull Request のブランチに対してのカバレッジを取得するステップです。  
カバレッジを取得して、フィーチャーブランチ用の octocov 設定を利用してレポートしています。  
2 つレポートすることで差分表示されるようになります。
