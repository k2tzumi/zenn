---
title: "【Day21】CDPを使ってブラウザ操作してみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-21 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/21配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして3週間が経ちました。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[DBに結果を格納する](https://zenn.dev/katzumi/articles/runn-tutorial-day20)」でした。

## CDPを使ってブラウザ操作してみる

CDPとは `Control browser using Chrome DevTools Protocol` になります。

https://chromedevtools.github.io/devtools-protocol/

これを使うと、ヘッドレスChromeでブラウザテストができるようになります！

さっそく見ていきましょう。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml


まずはrunnersの設定から

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L2-L3

これはもうおまじないだと考えてください。

ステップごとに説明していきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L7-L17

`actions` セクションでブラウザの操作手順を配列で指定します。

1. navigate: https://zenn.dev/  
URLをzenn.devにしてアクセスします
1. click: '#header-search'  
指定したセレクターの要素をクリックします。 `#header-search` は実際にはヘッダーにある虫眼鏡になります。
1. waitVisible: '#input-search-form'  
指定したセレクターの要素が表示されるまで待ちます。`#input-search-form` は実際には検索窓になります。
1. location
今アクセスしているページのURLを取得します。
`current.url` にURLが格納されます。

流れ的にはTOPページを開いて虫眼鏡をクリックして検索窓が表示されたURLが変わることを検証しています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L18-L32

次に検索ワードを入力するステップになります。
CDPの新しい操作は以下の通りなります。

1. sendKeys  
キー入力を行います。`sel`でセレクターを指定し、`input[id=input-search-form]`は実際には検索窓のinputフォームになります。
`value` に入力するワードを指定しています。変数から展開しています。
1. wait  
`time` は停止する時間を指定します。

一連の処理で、検索窓に変数（ `vars.keyword` ）を入力してURLが変わることを検証しています。
変数の使い方は ["変数を使ってみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day05) を参照ください

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L33-L39

CDPの新しい操作は以下の通りなります。

1. screenshot  
ページイメージをキャプチャします

キャプチャしたイメージのデータは `current.png` に格納されます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day21/cdp.yml#L40-L44

最後に前のステップでキャプチャしたイメージをdumpで出力しています。
`dump` によるファイル保存の方法は ["レスポンスをdumpして外部JSON化する"](https://zenn.dev/katzumi/articles/runn-tutorial-day17) を参照してください。

通常実行ではヘッドレスモードでブラウザが動作するので中身の挙動がわかりません。
CDPのactionsの調整をしているときにブラウザの挙動を確認したい場合にヘッドフルモードで実行する場合は環境変数（ `RUNN_DISABLE_HEADLESS=1` ）を指定してください。

```console
$ RUNN_DISABLE_HEADLESS=1 runn run day21/cdp.yml --verbose
=== CDPを使ってブラウザ操作してみる (day21/cdp.yml) ... ok
    --- 検索アイコンをクリック (startSearch) ... ok
    --- キーワードを入力してEnter (inputKeyword) ... ok
    --- スクリーンイメージをキャプチャする (captureScreen) ... ok
    --- スクリーンショットを保存する (saveScreenShot) ... ok


1 scenario, 0 skipped, 0 failures
```

ブラウザテストもAPIテストと同様に簡単にできることがご理解頂けたかと思います。　 


明日は「CIに組み込んでみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day22