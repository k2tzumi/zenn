---
title: "CDPを使ってブラウザ操作してみる"
free: true
---

# CDPを使ってブラウザ操作してみる

CDP とは `Control browser using Chrome DevTools Protocol` になります。

https://chromedevtools.github.io/devtools-protocol/

これを使うと、ヘッドレス Chrome でブラウザテストができるようになります！

さっそく見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml


まずは runners の設定から。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L2-L3

これはもうおまじないだと考えてください。

ステップごとに説明していきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L7-L17

`actions` セクションでブラウザの操作手順を配列で指定します。

1. navigate: https://zenn.dev/  
URL を zenn.dev にしてアクセスします。
1. click: '#header-search'  
指定したセレクターの要素をクリックします。 `#header-search` は実際にはヘッダーにある虫眼鏡です。
1. waitVisible: '#input-search-form'  
指定したセレクターの要素が表示されるまで待ちます。`#input-search-form` は実際には検索窓です。
1. location
今アクセスしているページの URL を取得します。
`current.url` に URL が格納されます。

流れ的には TOP ページを開いて虫眼鏡をクリックして検索窓が表示された際の URL の遷移先を検証しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L18-L32

次に検索ワードを入力するステップになります。
CDP の新しい操作は以下の通りなります。

1. sendKeys  
キー入力します。`sel` でセレクターを指定し、`input[id=input-search-form]` は実際には検索窓の input フォームになります。
`value` に入力するワードを指定しています。変数から展開しています。
1. wait  
`time` は停止する時間を指定します。

一連の処理で、検索窓に変数（`vars.keyword`）を入力して URL が変わることを検証しています。
変数の使い方は ["変数を使ってみる"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/env) を参照ください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L33-L39

CDP の新しい操作は以下の通りなります。

1. screenshot  
ページイメージをキャプチャします。

キャプチャしたイメージのデータは `current.png` に格納されます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L40-L44

最後に前のステップでキャプチャしたイメージを dump で出力しています。
`dump` によるファイル保存の方法は ["レスポンスをdumpして外部JSON化する"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/dump) を参照してください。

通常実行ではヘッドレスモードでブラウザが動作するので中身の挙動がわかりません。
CDP の actions の調整をしているときにブラウザの挙動を確認したい場合にヘッドフルモードで実行する場合は環境変数（`RUNN_DISABLE_HEADLESS=1`）を指定してください。

```console
$ RUNN_DISABLE_HEADLESS=1 runn run day21/cdp.yml --verbose
=== CDPを使ってブラウザ操作してみる (day21/cdp.yml) ... ok
    --- 検索アイコンをクリック (startSearch) ... ok
    --- キーワードを入力してEnter (inputKeyword) ... ok
    --- スクリーンイメージをキャプチャする (captureScreen) ... ok
    --- スクリーンショットを保存する (saveScreenShot) ... ok


1 scenario, 0 skipped, 0 failures
```

ブラウザテストも API テストと同様、簡単にできることがご理解頂けたかと考えます。 

