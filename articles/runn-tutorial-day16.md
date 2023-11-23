---
title: "【Day16】JSONファイルにパラメータ埋め込みをしてみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-16 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の12/16配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートして絶賛掲載中です。  
本記事はAPIシナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25日全部理解したら一人でrunnを使ってAPIシナリオテストや、ちょっとしたAPIと連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

前日の記事は　「[リクエストJSONを外部ファイル化する](https://zenn.dev/katzumi/articles/runn-tutorial-day15)」でした。

## JSONファイルにパラメータ埋め込みをしてみる

[昨日の記事](https://zenn.dev/katzumi/articles/runn-tutorial-day15) でリクエスト内容を外部ファイル（JSON）化できるようになりました。 
今回は、1つのJSONファイルで柔軟にデータパターンを増やせる方法について紹介したいと思います。

前回のシナリオの内容は大きく挙動を変えずにincludeを組み合わせたシナリオにしています。

* json-template.yml  
メインシナリオファイルです
* post-scrap.yml  
前回のスクラップ記事を登録するシナリオを分割しています。
* scrap.json.template  
今回の肝となるファイルです

https://github.com/k2tzumi/runn-tutorial/blob/main/day16/json-template.yml

変更があるステップを見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day16/json-template.yml#L9-L15

スクラップ記事の登録処理がincludeに書き直されています。
前回はvarsセクションでjsonファイルを読み込むようになっていましたが、include時のvarsでもjson展開ができるようになっています。
こちらの記述方法は ["includeしたシナリオの変数を書き換えよう"](https://zenn.dev/katzumi/articles/runn-tutorial-day13) を参照ください。

あと新しい要素として組み込み関数 `faker.UUID()` を呼び出しています。
こちらでダミーのUUIDを発行できます。
runnには他にも便利な組み込み関数が幾つかあるのでオフィシャルのドキュメント（ `Additional built-in functions` ）を参照ください。

https://github.com/k1LoW/runn#additional-built-in-functions

次の2,3ステップ目は前回とほぼ同じなので割愛して、post-scrap.ymlを見ていきます。


https://github.com/k2tzumi/runn-tutorial/blob/main/day16/post-scrap.yml

単純に前回の1ステップ目をそのまま切り出した感じになあります。

次にメインの scrap.json.template を見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day16/scrap.json.template

中身はJSONのファイルですが、パラメータの埋め込みがされています。

`{{.vars.title_suffix }}`

こちらの記述方法は [Go Template](https://pkg.go.dev/text/template) になります。
Go Templateとして認識させるには　`.json.template` という拡張子にする必要があります。　　

Template自体の記法については詳しくは説明しませんが、こちらの記述で `title_suffix` 変数が展開されます。
include元でダミーのUUIDが指定されているものが出力されることになります。

どういう値が展開されているかはdebugオプションを有効にして実行してみてください。

```console
% SESSION="miserarenaiyo" runn run day16/json-template.yml --verbose
=== JSONテンプレートを利用してデータの柔軟性を高めましょう (day16/json-template.yml) ... ok
    --- スクラップ記事を登録します (postScrap) ... ok
        === JSONリクエストを送信するシナリオ (day16/post-scrap.yml) ... ok
            --- スクラップ記事を登録します (postScrap) ... ok
    --- スクラップ記事をアーカイブします (archiveScrap) ... ok
    --- スクラップ記事を削除します (deleteScrap) ... ok


1 scenario, 0 skipped, 0 failures
```

SESSIONに何を指定させるか？は ["リクエストJSONを外部ファイル化する > 前準備"](https://zenn.dev/katzumi/articles/runn-tutorial-day15#前準備) を参照ください。

Go Templateを使うと、一部値が違うデータを用意したりすることが出来るので大変便利です。

明日は「レスポンスをdumpして外部JSON化する」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day17