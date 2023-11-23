---
title: "【Day15】リクエストJSONを外部ファイル化する / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-15 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/15配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして絶賛掲載中です。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[include専用シナリオを活用する](https://zenn.dev/katzumi/articles/runn-tutorial-day14)」でした。

## リクエストJSONを外部ファイル化する

今までの記事では参照系のAPI呼び出しばっかりでしたが、ここから更新・登録系のAPIも使いより実践的な内容にしていきたいと思います。 

### 前準備

本記事から更新系の処理が入ります。サンプルとしているzennは公開APIではなくAPIトークンの仕組みがないので、セッションをそのまま使います。
そのため、セッションCookieを特定する作業が必要となります。
zenn.devにログインした状態でDevTools等から `_zenn_session` というセッションCookieを確認してください。

![Cookie確認イメージ](/images/articles/runn-tutorial/copy-cookie.png =800x)

:::message
お使いのブラウザに合わせてCookieを確認してください。
上述はChromeでの例となります
:::

### 本題

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml


新しい要素を順番に説明していきたいと思います。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L2-L5

HTTP Runnerの設定で階層が追加されており、 `endpoint` と `useCookie` が存在すると思います。
`endpoint`は以前のエンドポイントの設定で、`useCookie` はCookieを利用するオプションとなります。
API側でCookieを出力する場合はこちらのオプションを有効にしてください。


次にvarsセクションになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L6-L8

`request` 変数の値が `json://scrap.json` になっています。
こちらが本記事のメインの内容になります。
こちらの記述で

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/scrap.json

外部ファイルのjsonの内容が変数の値として代入されます。
jsonのパスの指定ですが、記述しているrunbookからの相対パスで記述する必要があります。
絶対パスでも記載できますが、シナリオとデータを管理する上では相対パスの方が管理しやすいです。

ステップを順に見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L10-L25

こちらで外部ファイルのリクエスト内容を送信してスクラップ記事の登録を行っています。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L15-L16

CookieはHttpヘッダーとしてセットする必要があります。
複数Cookieの場合は、`cookie1=cookievalue1; cookie2=cookievalue2` といった形でセミコロンで区切ります。


https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L17-L18

リクエストBodyへのjsonの展開はこんな形になります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L24-L25

後続のステップの為に値に別名をつけています。
詳細は　["任意の値に別名をつける"](https://zenn.dev/katzumi/articles/runn-tutorial-day10) を参照ください。


https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L26-L39

2ステップ目でスクラップ記事をアーカイブしています。PUTでarchivedのステータスを更新できるようです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L40-L49

記事をアーカイブすると削除できるので3ステップ目で削除しています。
スクラップ記事の削除はDELETEメソッドを使うようです。
DELETE後は204(`No Content`)になります。

最後に実行コマンドとその結果になります。
前述のCookieの内容をSESSIONという環境変数に指定して以下のようなコマンドになります。

```console
% SESSION="miserarenaiyo" runn run day15/json.yml --verbose 
=== リクエストJSONを外部ファイル化できます (day15/json.yml) ... ok
    --- スクラップ記事を登録します (postScrap) ... ok
    --- スクラップ記事をアーカイブします (archiveScrap) ... ok
    --- スクラップ記事を削除します (deleteScrap) ... ok


1 scenario, 0 skipped, 0 failures
```

:::message
Cookieは秘匿情報なので見せられないので `miserarenaiyo` にしています。
ダブルコーテーションで囲む必要があります。
:::

環境変数の使い方については ["変数を外部から与えててみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day06) を参照ください。

如何でしたでしょうか？
サンプルとしてJSON化のメリットとしては薄いと感じるかもですが、複雑で大きいリクエストを扱う場合はyamlで記載するよりもJSONのまま扱った方が楽なケースもああります。
また、別のシナリオで同じリクエストデータとして扱えるのもメリットとなります。


明日は「JSONファイルにパラメータ埋め込みをしてみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day16