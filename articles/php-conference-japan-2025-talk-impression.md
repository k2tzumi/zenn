---
title: "PHP Conference Japan 2025参加＆登壇レポート"
emoji: "🐼"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phpcon","conference","iwillblog"]
published: true
---

## PHP Conference Japan 2025に参加＆登壇してきました

今年も、[PHP Conference Japan 2025](https://phpcon.php.gr.jp/2025/) に参加し、**初の登壇**も果たしてきました！
この記事では、参加したセッションの感想や、自身の登壇についてレポートします。各セッションのより詳細な内容やリアルタイムの感想は、私の X（旧 Twitter）のスレッドにまとめていますので、そちらも合わせてご覧ください。

## 聞いたセッション

* [yieldが変えるLaravelの世界：LazyCollection徹底入門](https://fortee.jp/phpcon-2025/proposal/fcae051f-5b13-4e08-989b-721edadb7134)  
yield は結構使っている方なので、歴史から Laravel の LazyCollection までの話を聞けて良かったです
[google スライド](https://docs.google.com/presentation/d/1YwtSoGs2yBqxwoYyAVAbdF5NKuzMs2Skv5xI_eBsP7U/edit?slide=id.g369d67fc3b9_1_47#slide=id.g369d67fc3b9_1_47)  
yield
  * [X の実況スレッド](https://x.com/katzchum/status/1938777669849751833)

* [エラーハンドリングはtry-catchだけじゃない！Result型で“失敗”を型にするPHPコードの書き方](https://fortee.jp/phpcon-2025/proposal/196d87d1-5cb4-437a-b063-d523096d4ae4)
ジェネリクスで Result 型を定義して扱うという実践的な内容や、ビジネスエラーと技術的エラーを分けて扱うというルールは良さそうと思いました
    @[speakerdeck](312698eaccb043ba8f28f1b48c4a4bf2)
  * [X の実況スレッド](https://x.com/katzchum/status/1938785887028236360)

* [PHP 8.4の新機能「プロパティフック」から学ぶオブジェクト指向設計とリスコフの置換原則](https://fortee.jp/phpcon-2025/proposal/1358c8dc-52af-479c-b50b-89dd21056841)
登壇後だったので、途中から in しました
    @[speakerdeck](3dff5bc426b241a5a48a15d84936b89e)
  * [X の実況スレッド](https://x.com/katzchum/status/1938831897741926872)

* [イベントストーミング図からコードへの変換手順](https://fortee.jp/phpcon-2025/proposal/7368450d-070c-4d23-a12f-37371d5c7947)
イベントストーミングに落とし込んで更新がない世界は AI にも優しいという理解をしました
    @[speakerdeck](19a186cf89904f63ad03f609dcda681a)
  * [X の実況スレッド](https://x.com/katzchum/status/1938839186041454994)


* [設計やレビューに悩んでいるPHPerに贈る、クリーンなオブジェクト設計の指針たち](https://fortee.jp/phpcon-2025/proposal/96c52734-c8e0-433f-b556-cc19b90c3d35)
BASE さんのリアーキテクチャの 3 部作の最後の章でした。具体的なクリーンなコードの書き方を丁寧にまとめられていて、学べるスライドでした
    @[speakerdeck](edbbd446abd346969f5d8f5cd20b3ae4)
  * [X の実況スレッド](https://x.com/katzchum/status/1938846315489960055)

* [PostgreSQLのRow Level SecurityをPHPのORMで扱う Eloquent vs Doctrine](https://fortee.jp/phpcon-2025/proposal/80764564-29d0-4cf1-a126-7662c2ad6cac)
ちょうど RLS が気になっていたので、個人的にタイムリーなセッションでした。質問もできて満足でした
    @[speakerdeck](ad9c0fc9c6ef47f190ee03d199587a14)
  * [X の実況スレッド](https://x.com/katzchum/status/1938854832380785021)


## 登壇

「モブワークによる SECI モデルの実践」というタイトルでトークしてきました。  
2020 年からモブをやってい来た中で、最近 SECI モデルという概念を知って、意識して取り組んできました。  
取り組んだ結果、いい感じになってきたので、ぜひ言語化したいと思いました。　　
プロポーザルを提出した所、機会を頂いたという流れでした。

@[docswell](https://www.docswell.com/s/katzumi/ZM79VX-practice-seci-with-mob)

ここから裏話  

登壇後、懇親会などで「AI と会話しながら開発を進めるのは、モブプログラミングに似ている」という興味深い意見を複数いただきました。  
実際にモブワークの中で AI も活用していますが、今のところ人と AI の間に隔たりはあまりないという個人的な感想です。  
でも人に聞く前に AI に聞いてしまうのは良くないなーというのはありますｗ  
AI が効果的な「ラバーダック」となり、アイデアの壁打ちや、アイデアを広げる役割になってくれている感じです。

SECI モデルを知ったタイミング

https://x.com/katzchum/status/1786935740297007603

技術書典でお話させて頂いた @panda_program さんにトーク後のディスカッションをさせて頂き、尚且つサインまで頂けて凄く満足でした。

https://x.com/Panda_Program/status/1938842015833432151

モブを始めた年の振り返り記事  
ずっとやり続けてきたなーという印象

https://zenn.dev/litalico/articles/36bdc74887543c

スライドのページ数が多くなってなくなく削ったものもありました。付録ができそう感じでしたｗ  

モブ自体に興味があるけれど、やったことがない人が多そうな印象だったので、リモート・モブプログラミングのテクニックをカジュアルにまとめてあげると良さそうと感じました。