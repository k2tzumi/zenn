---
title: "JSONファイルにパラメータ埋め込みをしてみる"
free: true
---

# JSONファイルにパラメータ埋め込みをしてみる

[前のチャプタ](https://zenn.dev/katzumi/books/runn-tutorial/viewer/json) でリクエスト内容を外部ファイル（JSON）化できるようになりました。 
今回は、1 つの JSON ファイルで柔軟にデータパターンを増やせる方法について紹介していきます。

前回のシナリオの内容は大きく挙動を変えずに include を組み合わせたシナリオにしています。

* json-template.yml  
メインシナリオファイルです。
* post-scrap.yml  
前回のスクラップ記事を登録するシナリオを分割しています。
* scrap.json.template  
今回の肝となるファイルです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day16/json-template.yml

変更があるステップを見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/main/day16/json-template.yml#L9-L12

こちらのステップで今回パラメータ埋め込みに利用する UUID を発行しています。  
UUID の発行には組み込み関数 `faker.UUID()` を呼び出しています。  
こちらでダミーの UUID を発行できます。  
runn には他にも便利な組み込み関数が幾つかあるのでオフィシャルのドキュメント（`Additional built-in functions`）を参照ください。  

https://github.com/k1LoW/runn#additional-built-in-functions

発行した値を変数に束縛しています。  
こちらの記述方法は ["任意の値に別名をつける"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/bind) を参照ください。

https://github.com/k2tzumi/runn-tutorial/blob/main/day16/json-template.yml#L13-L18

次にスクラップ記事の登録処理が include に書き直されています。
前回は vars セクションで json ファイルを読み込むようになっていましたが、include 時の vars でも json 展開ができるようになっています。
こちらの記述方法は ["includeしたシナリオの変数を書き換えよう"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/include-vars) を参照ください。

次の 2,3 ステップ目は前回とほぼ同じなので割愛して、post-scrap.yml を見ていきます。


https://github.com/k2tzumi/runn-tutorial/blob/main/day16/post-scrap.yml

単純に前回の 1 ステップ目をそのまま切り出した感じになります。

次にメインの scrap.json.template を見ていきます。

https://github.com/k2tzumi/runn-tutorial/blob/aa7a7557a1727299347cfa818a098dc98667e4c5/day16/scrap.json.template

中身は JSON のファイルですが、パラメータの埋め込みがされています。

`{{.title_suffix }}`

こちらの記述方法は [Go Template](https://pkg.go.dev/text/template) になります。
Go Template として認識させるには　`.json.template` という拡張子にする必要があります。

Template 自体の記法については詳しくは説明しませんが、こちらの記述で、先に束縛した `title_suffix` 変数が展開されます。
include 元でダミーの UUID が指定されているものが出力されることになります。

どういう値が展開されているかは debug オプションを有効にして実行してみてください。

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

SESSION に何を指定させるか？は ["リクエストJSONを外部ファイル化する > 前準備"](https://zenn.dev/katzumi/books/runn-tutorial/viewer/json#前準備) を参照ください。

Go Template を使うと、一部値が違うデータを用意したりすることが出来るので大変便利です。


