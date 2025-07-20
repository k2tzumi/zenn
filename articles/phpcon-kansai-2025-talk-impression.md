---
title: "PHPカンファレンス関西 2025参加＆登壇レポート"
emoji: "🐙"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phpcon","conference","iwillblog"]
published: true
---

## PHPカンファレンス関西 2025に参加＆登壇してきました

2025 年 7 月 18,19 日と開催された [PHPカンファレンス関西2025](https://2025.kphpug.jp/) に参加してきました！

この記事では、参加したセッションの感想や、自身の登壇についてレポートします。各セッションのより詳細な内容やリアルタイムの感想は、私の X（旧 Twitter）のスレッドにまとめていますので、そちらも合わせてご覧ください。

## Day0

* [PHPからはじめるコンピュータアーキテクチャ](https://fortee.jp/phpcon-kansai2025/proposal/f6be843a-5ba9-4798-8eba-a1207a9d1e85)
レイヤードアーキテクチャはフラクタル構造だという話を思い出しました  
単純が故に高速化のアプローチがいっぱいあって、実際はすごいロジックなんだろうなーという気持ちになりました
    @[speakerdeck](16e7559a024744a6aa58522db85f5fbf)
  * [X の実況スレッド](https://x.com/katzchum/status/1946117273279942879)

* [PHPUnitの限界をPlaywrightで補完するテストアプローチ](https://fortee.jp/phpcon-kansai2025/proposal/eaf0770d-8abb-41fa-be5b-7a2139185676)
PHPUnit で網羅性を担保し、Playwright でユーザー体験を保証するという凄く単純で明確な指針でやられているのが良かったです
    @[speakerdeck](e7018477e7a94d368c98c3ef25b89022)
  * [X の実況スレッド](https://x.com/katzchum/status/1946124863531614519)

* [抽象化という思考のツール -理解と実践-](https://fortee.jp/phpcon-kansai2025/proposal/e1684a11-eebe-480c-bca2-b847d329b227)
凄く丁寧に抽象化の話を体系立てられていて凄くすーっと入っていく感じでした。  
抽象化の往復のやり方をもっと深ぼって聞きたいと思いました。
  * [X の実況スレッド](https://x.com/katzchum/status/1946131503764975813)

## Day0 Night

[@panda_program](https://zenn.dev/panda_program) さんたちと、餃子をつまみながら、技術から組織論など幅広いお話をたくさんしました。

![餃子](/images/articles/phpcon-kansai-2025-talk-impression/dumplings.jpg)

明日もあるので軽めで解散しました。


## Day1 前半

* [ソフトウェア・デザインに向かおう ～　世界を（ちょっとだけ）変えるソフトウェアを目指して](https://fortee.jp/phpcon-kansai2025/proposal/5d2c3e17-c9b1-4287-a0cf-9762b58a385b)  
凄く示唆が富んでいて、考えさせられるセッションでした。  
テーブル駆動方式が複数のアクションが絡み合ったディシジョンテーブルなのでは？と感じました。
    @[speakerdeck](867cf85bd07d4f5daad40e2601b0b28a)
  * [X の実況スレッド](https://x.com/katzchum/status/1946379523270078560)

* [階層化自動テストで開発に機動力を](https://fortee.jp/phpcon-kansai2025/proposal/ea0b4340-31e3-4ca6-a151-505c8dc40c91)  
テストをレイヤー分けして考える為の軸としてフローと状態にわけて考えるというのは、なるほど！と感じました。  
チーム内でテスト目的を確認する場が結構あったりするので、積極的に共有していきたいと思いました。
    @[speakerdeck](2b2bfe68a3244657a8de2cce9c30d82d)
  * [X の実況スレッド](https://x.com/katzchum/status/1946393353169592596)

## 登壇

「PHP でやってみよう！テストだけじゃない、デシジョンテーブル（決定表）実装の勘所」というタイトルでトークしてきました。  
現在開発しているシステムのコアな実装部分について大公開しました。

@[docswell](https://www.docswell.com/s/katzumi/KLQL3Q-decision-table-implementation-tips)

元ネタとなったプロジェクトのメンバーが増員中で、如何に複雑な条件を認知負荷を上げることなく実装するか？という所を伝えたくて言語化しました。  

登壇後にスライド内の「直交表」という表現について、不適切ではないかとご指摘を受けました。  
確かに全パターンテストだったので、後で訂正をしたいと思います。

あと今回の会場の登壇体験がかなり良かったです。

https://x.com/katzchum/status/1946420290457595942

## Day1 後半

* [副作用と戦う PHP リファクタリング ─ ドメインイベントでビジネスロジックを解きほぐす](https://fortee.jp/phpcon-kansai2025/proposal/f1dec853-4070-44ad-8eda-ef8b127dda3f)  
ちょうど Laravel Events を調べている時に聞けて個人的にタイムリーでした。同期処理から初めて非同期へ切り替えていくという流れで考えるのが良いというのは、すごい腑に落ちました。  
    @[speakerdeck](ae912cda591d4e30b51678753a7e8249)
  * [X の実況スレッド](https://x.com/katzchum/status/1946432322875899919)

* [「次に何を学べばいいか分からない」あなたへ──若手PHPエンジニアのための学習地図](https://fortee.jp/phpcon-kansai2025/proposal/4f91183c-b4fc-4e0f-aa77-a83d83218fe0)  
t-wada 先生が仰っている「本同士の時系列，関係性，立ち位置を考えて読む」というのを実践されていて、またその感想も聞けたのが凄い良かったです。  
色々のエンジニアの方の書籍を辿られてきた事例を聞けると良さそうと感じました。
    @[speakerdeck](5654810124f24e8bbc0abbc9572095df)
  * [X の実況スレッド](https://x.com/katzchum/status/1946439851823583316)

* [PHPStanへのコントリビュートを通して更にPHPStanに詳しくなった話](https://fortee.jp/phpcon-kansai2025/proposal/1ee1c63d-c6b0-4e02-9f78-d0a727ef07f8)  
今回のケースが、IDE でよく表示されるビルトイン関数の型定義だと理解できたのは学びでした。  
コントリビュートもされていて、その体験も興味深い感じでした。
[google スライド](https://docs.google.com/presentation/d/17tVgyN83sqQP7DpDuRXXYDc9ZQE2c3O7Ei8_51gaWI8/edit?usp=sharing)  
  * [X の実況スレッド](https://x.com/katzchum/status/1946453051742863633)

## 懇親会 & 二次会

カンファレンスは熱い想いをもった人たちに支えられているなーと感じた一幕でした。

https://x.com/katzchum/status/1946783353489002809

懇親会で次の登壇ネタを頂いたので、福岡にはまた CfP を出したいです。  

![ビール](/images/articles/phpcon-kansai-2025-talk-impression/beer.jpg)

２次会では、この先生き残るには？という話を中心に熱く話していました。

## Day2（非公式）

https://php-lovers-meetup.connpass.com/event/354724/

OSS Gate を見学させていただきました。

https://x.com/katzchum/status/1946792281400643908

[@LuckyWind_sck](https://x.com/LuckyWind_sck) さんが今回 LT で登壇されていて Slidev を始めて使われたとのことで、Slidev 談義ができて良かったです。

