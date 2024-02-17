---
title: "ファイルをアップロードする"
free: true
---

# ファイルをアップロードする

ニッチなリクエストパターンとして multipart/form-data がありますが、今回はファイルアップロードをやっていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml

今回も Cookie を利用します。
Cookie を利用したシナリオについては ["リクエストJSONを外部ファイル化する"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/dump) にありますので、参照ください。

ステップ毎にみていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L9-L15

アップロード対象のファイルをダウンロードします。
ダウンロードには外部コマンドの curl を利用しています。
外部コマンドの実行については ["外部コマンドを実行してみる"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/exec) を参照ください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L16-L30

本記事のメインである multipart/form-data 形式によるリクエスト方法になります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L24-L25

body がいつもは `application/json` だったのが `multipart/form-data` となります。
multipart/form-data 以下は、パラメータを指定しますがファイルパスを指定すると、multipart にしてリクエスト送信されます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day23/file-upload.yml#L26-L30

こちらの画像アップロードの API は成功時に 201 となるようです。
文字列の比較で `startsWith` というのがありますが、こちらは先頭文字が同一であれば true になります。

以下は実行コマンドになります。`debug` オプションをつけてみると挙動がわかっていいです。

```console
% SESSION="miserarenaiyo" runn run --scopes run:exec day23/file-upload.yml --verbose 
=== ファイルアップロードする (day23/file-upload.yml) ... ok
    --- ファイルをダウンロードしてくる (downloadFile) ... ok
    --- ファイルをアップロードする (fileUpload) ... ok


1 scenario, 0 skipped, 0 failures
```
