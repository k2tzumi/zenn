---
title: "PHPカンファレンス福岡2024に参加してきました"
emoji: "👄"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phpcon","conference","phpconfuk","phpカンファレンス福岡","runn"]
published: true
---

## はじめに

月間 PHP カンファレンスの 6 月号である福岡に参加してきましたので、レポートします。

https://phpcon.fukuoka.jp/2024/

去年に引き続き今年も参加できて良かったです。
福岡には 6/20 〜 6/23 までお邪魔していました。

## 前夜祭

PRTIMES さん主催の [前夜祭](https://prtimes.connpass.com/event/319375/) にお邪魔してきました。

https://x.com/katzchum/status/1804066891574710534

去年は初めて会う人ばかりでしたが、あれから各地のイベントでお会いして大分顔とお名前を覚えられた感があってイベント慣れができたなぁと実感しました。

最初は yamato  さんのお話。

https://speakerdeck.com/yamato_sorariku/number-phpconkagawa-regasikodonimoobuzababiriteiwo-shao-sizutushi-merusabisujian-shi

香川のトーク内容を深ぼった内容（自主規制解除版）とのこと。（リンクも香川のものです）
最近はマネージドなサービスを利用することが多くなっているので大分使うことがすくなくなった cron ですが、やっぱり辛いですよねーという感想でした。
自分も昔苦しめられたので、徐々に改善していく流れをなるほどなーと思いながら見させていただいていました。

## PHPsakaba in 福岡

https://connpass.com/event/320648/


もつ鍋仙頭大名で美味しい料理を頂きつつ楽しいお話がたくさんできて良かったです。
話に夢中となってしまって、写真をほとんど撮ってなくて勿体無いことをしたなと思い出しました。
めんたい・鯖が新鮮で感動しました。

## PHPカンファレンス福岡

当日は生憎の雨でした。

https://x.com/katzchum/status/1804306431707140545


### SPLから始める「データ構造」入門

きんじょうさんの発表。

https://speakerdeck.com/o0h/phpconfuk-2024

すごく丁寧な説明をされていて流石でした。
単純に関数の説明だけでなく、読み手に訴えるコードということが個人的にすごい響きました。
普段は Laravel のコレクションを使っているので、SPL を使うタイミングがなかなかないのですが、機会があったら使ってみたいと思いました。


### 並行処理を学びGuzzleと仲良くなる

しまぶさんの発表。

https://speakerdeck.com/shimabox/bing-xing-chu-li-woxue-biguzzletozhong-liang-kunaru

しまぶさんに前夜祭でお会いして今回のセッションが大分難産だった雰囲気を感じたので、大変気になっていました。
並行処理・並列処理の違いの説明から始まり、「そう、そう」という感じになり、そわそわしてました。
Guzzle の非同期リクエスト（≠並行処理）は、複雑かつ挙動が期待していたのと異なり（ブロッキングしてしまう）挫折した経験がありました。
関数の内容を大変丁寧にまとめられていて Guzzle と仲良くできそうでした。
Ask the speaker では Fiber を使って真の並行処理を目指そうとしたことも含めて軽くお話させていただきました。
すごく刺激をもらえたのでどこかでチャレンジしてみたいと思いました。

### 有効な使い方を正しく理解して実装する PHP8.3の最新機能

清家さんの発表。

https://speakerdeck.com/seike460/proper-understanding-and-implementation-of-effective-usage-latest-features-in-php-8-dot-3

PHP8.3 から const で型が使える様になったということで聞いてきました。
直近ばたば t がしていたので、バージョンアップできてなかったのですが、そろそろバージョンアップしようと思いました。
Override とか地味だけれども便利そう。Rector で inheritDoc アノテーションがあったら、つけてくれるやつってあったら便利そうと思いました。

## LTアワー

https://x.com/katzchum/status/1804335139583070717

### パスワードよもやま話

shogogg さんの LT。

https://speakerdeck.com/shogogg/pasuwadoyomoyamahua

ベストプラクティスを知れたのが学びになりました！


### runn開発者会議福岡2024

勝手に開発者会議の開催宣言していました w。

@[docswell](https://www.docswell.com/s/katzumi/5P9VEX-runn-developers-conference-fukuoka-2024-keynote-speech)

LT 用にまとめて改めて気づきましたが、１年かけて 6 回開催できていることは大変ありがたいことだと思っています。

## 午後の部

### リモートワーク時代の守護神：PHP開発者のためのセキュリティ強化術

P 山さんの発表。

https://speakerdeck.com/pyama86/rimotowakushi-dai-noshou-hu-shen-phpkai-fa-zhe-notamenosekiyuriteiqiang-hua-shu

色々セキュリティのお話。
本題は PHP で PHPer の為の認証モジュールを作成してみたで、前夜祭で飲みすぎてデモまで完遂するという P 山さんの強さを感じたセッションでした w。

### PHPStanの中を知ってコントリビュートしよう

tadsan さんの発表。

https://tadsan.fanbox.cc/posts/8137393

リポジトリ群の説明から、Lucy commit という神々の遊びとかの紹介など、PHPStan をウォッチしてこられた凄いマニアックな内容に圧倒されました。
Phar で配布すると依存の衝突がしにくいというのはなるほどなーと思いました。

### Azureもなかなか良いですよ！ - PHPerの為のAzure入門

kaz さんの発表。

https://speakerdeck.com/kaz29/phpconfu-gang-2024-azuremonakanakaiitesuyo

Azure も一杯サービスがあるなーという感じでした。貴重な事例を紹介して頂いてありがたい内容でした。

### PHPでデータベースを作ってみた

hanhan さんの発表。

https://speakerdeck.com/hanhan1978/create-data-with-php

タイトルからしてドキュメントデータベースなのかな？と期待していたら、まさかの MySQL！
ノリで作るもんじゃないぞ w と思いながら見ていました。
プロトコル互換で作ろうとするアプローチはガチでした w
何故ベストを尽くしたのか？という思いでした。

### 設計の考え方 - インターフェースと腐敗防止層編

おかしょいさんの発表。

https://speakerdeck.com/okashoi/interface-and-anti-corruption-layer

よく聞くけれどあまり良くわかっていないワードの中で上位に来る腐敗防止層の話でした。
初学者にもわかりやすく解説されているので、腐敗防止層について質問されたらこのスライドを紹介すれば OK になったというのは本当にありがたいと思いました。

### PHPUnit 11 概論

02 さんの発表。

https://speakerdeck.com/cocoeyes02/introduction-of-phpunit-11

PHPUnit のバージョンアップを試みていた最中なので、非常にタイムリーでした。
expects が非推奨になった影響が大きそうと感じました。
Ask The Speaker で Attirbutes の covers についてお話をさせて頂いたりしました。

### PdMから見た「スーパーエンジニア」の思考と行動

https://speakerdeck.com/happy_imafuku/thoughts-and-actions-of-super-engineers-as-seen-from-pdm

つよつよなエンジニアに囲まれている PdM な方のお話。
普段から「それは How なんよ」という言葉が飛び交うチームというのはそれ自体が強いよなーと感じました。

### 【超特急】「SQLアンチパターン」 総おさらいLT 【5分で25個】

つざきさんの発表。

https://speakerdeck.com/ytsuzaki/phpkanhuarensufu-gang-2024-chao-te-ji-sqlantipatanzong-osarailt

会場も巻き込んでハイスピードでいく LT は惚れ惚れする感じでした。

### クロージング

実行委員長のびきニキさんのクロージングも感動的でした。
振り返り内で挙手確認されていましたが、初参加や学生さんが大変多かったので印象的でした。

https://note.com/012xx_/n/n38f1eb85dfb3

> コミュニティ全体の新陳代謝を促進する

目標を達成していて素晴らしいと感じたのと、実行委員長も含めて福岡の人材の層が厚いなーと感じました。

### 懇親会

色々な方とやんややんやとお話をさせて頂きました。
runn 開発者会議も少しながら話ができました。
主にクリーンアップ処理についていくつかの案についてディスカッションしました。
結論までには至らなかったのですが、オフラインでのやりとりはやっぱり尊いと感じました w
カンファレンスからしか摂取できない栄養があります。

２次会 3 次会までワイワイとしましたが、自身のエネルギー切れを感じてしまいました。というか周りのパワーがすごい w
ただ昨年、食べられなくて後悔した念願のラーソーメンを体験できて良かったです。

## 後夜祭

今年も Fusic さんの After Hack に参加させていただきました。

https://fusic.connpass.com/event/314905/

新しいイベントスペースだったらしく、すごく快適できした。
ディスプレイから PD 対応の type-C ケーブルが生えていて、フリーアドレスのお手本の様な環境で素晴らしかったです。
LT を聞きながら、昨日の runn 開発者会議で話をしたディスカッション内容を [まとめたり](https://zenn.dev/katzumi/scraps/f00a2d3e177b77)、溜まりに溜まった月間 PHP カンファレンスのブログをまとめるなどしていました。  
途中リサイタルが始まったり、お昼は参加メンバーとランチに行ったりしました。
飛行機の時間が来てしまったので、後ろ髪を引かれながら離脱しました。

# 最後に

今年は、6 月まで多忙を極めていたので、本当に参加できて良かったです。
また来年が楽しみとなるカンファレンスでした！

https://x.com/katzchum/status/1804806714648121825

来年は CfP を通したい！
あと天ぷらと烏賊を食べに行きたい！