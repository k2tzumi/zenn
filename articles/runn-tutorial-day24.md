---
title: "【Day24】runnを利用したちょっとしたツール作成してみる / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-24 00:00
---

:::message

この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/24 配信になります。

:::

## はじめに

一人アドベントカレンダーも後 2 日です。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日間のチュートリアルを経て、 runn を使っての API シナリオテストや、 API と連動させる自動化処理を一人で行えるようになることを目標にしています。 
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試し、もし躓いた箇所があれば、記事のコメント欄にお知らせいただけると幸いです。

前日の記事は「[ファイルをアップロードする](https://zenn.dev/katzumi/articles/runn-tutorial-day23)」でした。

## runnを利用したちょっとしたツール作成してみる

今日で実技としてのチュートリアルは完了となります。お付き合い頂いた方ありがとうございます！
今日は今までのチュートリアルで扱ったテクニックを総動員してちょっとしたツールを作成していきます。

今まで作ったステップを集結させているので、ファイル数が多いです。

* [final-task.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/final-task.yml)  
シナリオのメインです。
* [init-db.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/init-db.yml)  
データベース初期化シナリオ。
* [collect-article.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/collect-article.yml)  
記事一覧を DB 保存するシナリオ。
* [capture-article.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/capture-article.yml)  
記事ページのスクリーンショットを取得するシナリオ。
* [file-upload.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/file-upload.yml)  
ファイルアップロードを行うシナリオ。
* [crop-image.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/crop-image.yml)  
画像切り出しを行うシナリオ。
* [post-scrap.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/post-scrap.yml)  
スクラップ投稿するシナリオ。
* [post-comment.yml](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/post-comment.yml)  
コメント投稿するシナリオ。
* [comment.json.template](https://github.com/k2tzumi/runn-tutorial/blob/main/day24/comment.json.template)  
コメントのリクエストテンプレート。

メインの runbook はこちら。

https://github.com/k2tzumi/runn-tutorial/blob/main/day24/final-task.yml

内容は細かく説明しませんが、1 つ 1 つのステップについては、昨日までの記事で紹介したものになります。
それらを組み合わせしてシナリオが作成されています。

実行コマンドは以下の様になります。

```console
% SESSION="miserarenaiyo" runn run --scopes run:exec day24/final-task.yml --verbose
=== runnでここまで出来るというデモ (day24/final-task.yml) ... ok
    --- 1週間前の時間を取得する (calcDate) ... ok
    --- 最新記事を収集する (collectArticle) ... ok
        === 指定されたページの最新記事をDBに保存する (day24/collect-article.yml) ... ok
            --- スキーマ作成 (initSchame) ... ok
                === スキーマを定義する (day24/init-db.yml) ... ok
                    --- テーブル作成 (crateTable) ... ok
                    --- テーブル一覧を確認する (showTables) ... ok
            --- varsで指定された件数分、記事一覧を取得します (listArticles) ... ok
            --- 記事をテーブルに保存する (saveArticles) ... ok
    --- ランキングデータを作成する (makeRanking) ... ok
    --- 記事ページをキャプチャする (captchaArticle) ... ok
        === 指定された記事ページのスクリーンショットをキャプチャする (day24/capture-article.yml) ... ok
            --- 指定した記事ページを表示 (viewArticle) ... ok
            --- 指定した記事ページの本文をキャプチャ (captureArticle) ... ok
    --- 画像を切り出しする (cropImage) ... ok
        === 画像切り出しを行う (day24/crop-image.yml) ... ok
            --- 画像切り出しを行う (cropImage) ... ok
    --- 画像をアップロードする (fileUpload) ... ok
        === ファイルアップロードする (day24/file-upload.yml) ... ok
            --- ファイルをアップロードする (fileUpload) ... ok
    --- スクラップ記事を登録する (postScrap) ... ok
        === スクラップ投稿を行う (day24/post-scrap.yml) ... ok
            --- スクラップ記事を登録します (postScrap) ... ok
    --- コメントを投稿する (postComments) ... ok
        === コメント投稿を行う (day24/post-comment.yml) ... ok
            --- コメントを投稿する (postComments) ... ok


1 scenario, 0 skipped, 0 failures
```

今回も Cookie を使います。Cookie の利用方法（取得方法）は ["リクエストJSONを外部ファイル化する"](https://zenn.dev/katzumi/articles/runn-tutorial-day15) を参照してください。

こちらのシナリオを実行した結果がこちらになります。

https://zenn.dev/katzumi/scraps/2ea60934680341


API を利用しランキングページを自動生成しています！
画像の加工の為に環境依存のコマンドを利用しているのと、出力している Markdown は簡素なものですがここまで出来ました！

runn は API シナリオテストのツールとして印象が強いですが、自動化ツールでもあるということも、こちらのサンプルで実証できたと考えています。

API シナリオテスト以外の利用用途として以下の様なことも出来るのでは無いでしょうか？

* デプロイツール
* 監視ツール
* etc

明日は「runn のもっと便利な使い方を紹介」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day25