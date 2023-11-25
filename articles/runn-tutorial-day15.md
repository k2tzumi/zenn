---
title: "【Day15】リクエストJSONを外部ファイル化する / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-15 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/15 配信になります。

:::

## はじめに

一人アドベントカレンダーとしスタートして絶賛掲載中です。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[include専用シナリオを活用する](https://zenn.dev/katzumi/articles/runn-tutorial-day14)」でした。

## リクエストJSONを外部ファイル化する

今までの記事では参照系の API 呼び出しばっかりでしたが、ここから更新・登録系の API も使いより実践的な内容にしていきます。 

### 前準備

本記事から更新系の処理が入ります。サンプルとしている zenn は公開 API ではなく API トークンの仕組みがないので、セッションをそのまま使います。
そのため、セッション Cookie を特定する作業が必要となります。
zenn.dev にログインした状態で DevTools 等から `_zenn_session` というセッション Cookie を確認してください。

<!-- textlint-disable -->
![Cookie 確認イメージ](/images/articles/runn-tutorial/copy-cookie.png =800x)
<!-- textlint-enable -->

:::message

お使いのブラウザに合わせて Cookie を確認してください。
上述は Chrome での例となります。

:::

### 本題

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml


新しい要素を順番に説明していきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L2-L5

HTTP Runner の設定で階層が追加されており、 `endpoint` と `useCookie` が存在します。
`endpoint` は以前のエンドポイントの設定で、`useCookie` は Cookie を利用するオプションとなります。
API 側で Cookie を出力する場合はこちらのオプションを有効にしてください。


次に vars セクションになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L6-L8

`request` 変数の値が `json://scrap.json` になっています。
こちらが本記事のメインの内容になります。
こちらの記述です。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/scrap.json

外部ファイルの json の内容が変数の値として代入されます。
json のパスの指定ですが、記述している runbook からの相対パスで記述する必要があります。
絶対パスでも記載できますが、シナリオとデータを管理する上では相対パスの方が管理しやすいです。

ステップを順に見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L10-L25

こちらで外部ファイルのリクエスト内容を送信してスクラップ記事を登録します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L15-L16

Cookie は Http ヘッダーとしてセットする必要があります。
複数 Cookie の場合は、`cookie1=cookievalue1; cookie2=cookievalue2` といった形のセミコロン区切りとなります。


https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L17-L18

リクエスト Body への json の展開はこんな形になります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L24-L25

後続のステップの為、値に別名をつけています。
詳細は　["任意の値に別名をつける"](https://zenn.dev/katzumi/articles/runn-tutorial-day10) を参照ください。


https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L26-L39

2 ステップ目でスクラップ記事をアーカイブしています。PUT で archived のステータスを更新できるようです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day15/json.yml#L40-L49

記事をアーカイブすると削除できるので 3 ステップ目で削除しています。
スクラップ記事の削除は DELETE メソッドを使うようです。
DELETE 後は 204(`No Content`)になります。

最後に実行コマンドとその結果になります。
前述の Cookie の内容を SESSION という環境変数に指定して以下のようなコマンドになります。

```console
% SESSION="miserarenaiyo" runn run day15/json.yml --verbose 
=== リクエストJSONを外部ファイル化できます (day15/json.yml) ... ok
    --- スクラップ記事を登録します (postScrap) ... ok
    --- スクラップ記事をアーカイブします (archiveScrap) ... ok
    --- スクラップ記事を削除します (deleteScrap) ... ok


1 scenario, 0 skipped, 0 failures
```

:::message

Cookie は秘匿情報なので見せられない為 `miserarenaiyo` にしています。
ダブルコーテーションで囲む必要があります。

:::

環境変数の使い方については ["変数を外部から与えてみる"](https://zenn.dev/katzumi/articles/runn-tutorial-day06) を参照ください。

如何でしたでしょうか？
サンプルとして JSON 化のメリットとしては薄いと感じるかもですが、複雑で大きいリクエストを扱う場合は yaml で記載するよりも JSON のまま扱った方が楽なケースもああります。
また、別のシナリオで同じリクエストデータとして扱えるのもメリットとなります。


明日は「JSON ファイルにパラメータ埋め込みをしてみる」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day16