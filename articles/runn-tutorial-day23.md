---
title: "【Day23】ファイルをアップロードする / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-23 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/23 配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートしてそろそろラストスパートです。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日全部理解したら一人で runn を使って API シナリオテストや、ちょっとした API と連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は「[CIに組み込んでみる](https://zenn.dev/katzumi/articles/runn-tutorial-day22)」でした。

## ファイルをアップロードする

ニッチなリクエストパターンとして multipart/form-data がありますが、今回はファイルアップロードでやってみたいと思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml

今回も Cookie を利用します。
Cookie を利用したシナリオについては ["リクエストJSONを外部ファイル化する"](https://zenn.dev/katzumi/articles/runn-tutorial-day15) にありますので、参照ください。

ステップ毎にみていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L9-L15

アップロード対象のファイルをダウンロードを行っています。
ダウンロードには外部コマンドの curl を利用しています。
外部コマンドの実行については ["外部コマンドを実行してみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day19) を参照ください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L16-L30

本記事のメインである multipart/form-data 形式によるリクエスト方法になります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L24-L25

body がいつもは `application/json` だったのが `multipart/form-data` となります。
multipart/form-data 以下は、パラメータを指定しますがファイルパスを指定すると、multipart にしてリクエスト送信されます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L26-L30

こちらの画像アップロードの API は成功時は 201 になるようです。
文字列の比較で `startsWith` というのがありますが、こちらは先頭文字が同一であれば true になります。

以下は実行コマンドになります。`debug` オプションをつけてみると挙動がわかっていいです。

```console
% SESSION="miserarenaiyo" runn run --scopes run:exec day23/file-upload.yml --verbose 
=== ファイルアップロードする (day23/file-upload.yml) ... ok
    --- ファイルをダウンロードしてくる (downloadFile) ... ok
    --- ファイルをアップロードする (fileUpload) ... ok


1 scenario, 0 skipped, 0 failures
```

明日は「runn を利用したちょっとしたツール作成してみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day24