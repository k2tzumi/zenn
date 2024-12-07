---
title: "runn開発者会議 in PHPerKaigi 2023"
emoji: "🐘"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phperkaigi","conference","iwillblog","runn"]
published: true
---

## はじめに

ブログを書くまでがカンファレンスですということで [PHPerKaigi2023](https://phperkaigi.jp/2023/) の参加の体験記事的なやつです。  
なにげにオフラインでの参加は初でした。  

初めて参加しようとなったきっかけは以下のやり取りでありました

https://twitter.com/k1LoW/status/1635622359808999425

![やりとり](/images/articles/runn-developers-conference-in-phperkaigi2023/tweet.png)

記事のタイトルにある runn というツールの開発者である [@k1low](https://twitter.com/k1low) さんと初顔合わせの実績を解除できました！  
runn って何？という方は以下の記事を参照ください。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

以下に印象に残ったセッションや出来事をピックアップしてまとめます。

## Day0

お仕事だったのでオンラインで途中参加

■PHPUnit 10 概論
https://speakerdeck.com/cocoeyes02/introduction-of-phpunit-10

ホットな話題でありがたかったです。
以前使えていたいくつかの関数が使えなくなっているらしいです。
今携わっているプロジェクトで適用済みだけれど dataProvider 以外はそれほど苦労しなかったけれど、使い込んでいるプロジェクトでは対応が大変そう。

■アンドキュメンテッド「ちょうぜつソフトウェア設計入門」
https://speakerdeck.com/tanakahisateru/andokiyumentetudo-tiyouzetusohutouea-she-ji-ru-men-obuziekutozhi-xiang-niding-yi-hanai-noka

個人的にツボにはまったセッションでした。
ソフトウェア開発の歴史をまとめていくと色々見えてくるものがあるのでは？という良い気付きになりました。  
技術の進化の歴史は振り子ではなく螺旋である ^[https://speakerdeck.com/twada/understanding-the-spiral-of-technologies] という話があります。その螺旋の差分を抑えるには歴史から学ぶ必要があると感じました。


## Day1

■PHP で学ぶ "Cache の距離" の話
https://speakerdeck.com/hanhan1978/study-cache-with-php

一番最初はキャッシュのお話。  
キャッシュの種類が２つあり、そキャッシュする目的と手段が異なってくることを丁寧に説明されていました。
PHP のシリアライザーがバージョンによって挙動が変わらないかなという疑問がありましたが、最初のセッションで質問するハードルが高くて聞けませんでした。

■Win Testing Trophy Easily / テスティングトロフィーを獲得する
https://speakerdeck.com/k1low/phperkaigi-2023

テスティングトロフィーという用語を初めて知りました。
アーキテクチャだけ決まっていて、テスト戦略を考えた結果 API シナリオテストに辿り着いという話は、runn が生み出される重要な動機になっているので大変興味深く聞きました。  
API と DB のスキーマは変化が少ないという着眼点し、長期的に維持し易いテストスイートを目指したという話は、自分が実際に runn を使っていてコスパが良いと実感できているので、狙い通りで撃ち抜かれたと思いましたｗ  
すべてのバックエンドエンジニアには是非 runn を使ってもらいたいと感じています。  
最近は SPA なプロダクトが多くてフロントも含めて E2E テストしたいと感じている方も多いかと思います。  
ただ E2E テストの導入のハードルの高さや、始めたけれどテストがすぐ壊れて維持できず断念した方も多いのではないでしょうか。私も実際に検討しましたが諦めてしまっていました。  
runn を使った API シナリオテストであれば、学習コストや導入手間がほとんどなく CI まで組み込むことが可能です。  
フロントの実装が完了していない状態で API テストを Postman や curl コマンドなどで手動で確認したことがある場合は、ぜひ runn で API を呼び出してみてください。直ぐに気に入ると思います。  
作ったシナリオファイルは優秀な動くドキュメントになるので、フロントチームが独立している場合はドキュメント代わりに是非書いてみることをオススメします。  
おまけのスライドでも触れられていますが、deploy 直後のリグレッションテストにそのまま使えるのでポチポチテストを代替させることも出来ます。一粒で 3 度ぐらい美味しいです w

■QA
Q. 1つのシナリオファイルで複数データパターンを扱うことは出来ますか？
A. json ファイルを読み出してデータパターン分繰り返し実行できます。
このデータ駆動テストの機能拡張を行ってくれたコントリビューターが本日会場に来てくれていると思います（ニヤリ

https://twitter.com/katzchum/status/1639098721744080897

はい。私ですｗ

■Ask the Speaker
先に 3 名ぐらいお話をされていて、落ち着いた頃を見計らって初めてのご挨拶をさせていただきました。  

軽く感想や雑談をしてから runn 談義がスタートしました。  
しばらくしてから k1low さんから以下の Issue について話をしたいと頂きました。

https://github.com/k1LoW/runn/issues/397

結構以前に作成した Issue だったので、実現したいこと・具体的なユースケースについて説明をしました。  
普段は Issue や PR でやり取りしていましたが、やはりダイレクトにネイティブ言語でやりとりできるといいですね。

■防衛的 PHP: 多様性を生き抜くための PHP 入門 
https://speakerdeck.com/siketyan/defensive-php
PHPStorm が CI で動くんですか！？
良いことを聞いたので試したいと思います。


■k1low さんからの FB

https://twitter.com/k1LoW/status/1639128685856067584

Ask the Speaker で話をしていた件につい早速フィードバックいただきました。


■安全にプロセスを停止するためにシグナル制御を学ぼう！ 
https://speakerdeck.com/yamamotohiroya/an-quan-nihurosesuwoting-zhi-surutamenisikunaruzhi-yu-woxue-hou
このセッションを聞いて、Laravel Queue worker を安全に止める方法が気になって調べてみましたが、オフィシャルで停止コマンドが用意されていなくて少しびっくりしています。  
安全に止める方法をご存知の方は教えていただけると嬉しいです。


■Rector ではじめる "運用を止めない" PHP アップグレード
https://fortee.jp/phperkaigi-2023/proposal/d3084a8f-89b5-48d6-8a81-59c460b3b3e0
スライド見つからず

Rector を最近導入していたので興味津々でした。  
Rector を CI に組み込んだことで、非互換のコードが混入しないようになったという話になるほどーと感心しました。 
目的が達成されたら Rector を削除したというのも良いと思いました。


■LT 大会（6 本）

みんな勢いがあって凄い w

■推測するな、計測せよ New Relic × Laravel 実践
https://speakerdeck.com/mpyw/tui-ce-suruna-ji-ce-seyo-new-relic-x-laravel-shi-jian
オンライン登壇なのに会場で本人解説付きで面白かったです w

■池袋 Night

https://twitter.com/k1LoW/status/1639205573316206598

本当に色々なお話をさせていただきました。  
貴重な話で内容が濃く、Podcast にすると面白そうと感じました。

## Day2

午前中は用事があり午後から参加

■k1low さんのフットワークの軽さ

https://twitter.com/k1LoW/status/1639453320908779522

■アンカンファレンス

無限 LT が開始されていたので覗いていました。  
現同僚と元同僚がそれぞれ LT で発表していた。


■Attribute を極める
https://fortee.jp/phperkaigi-2023/proposal/86a02001-4409-49c9-9fbe-4db251c96cb0

Attribute ネタを継続的に発信して頂けているの有り難いです。  


■runn 開発者会議 2 日目

https://twitter.com/k1LoW/status/1639543221553606657

ホワイトボードも使えるのもリアルならではです。

■LT 大会 2 日目

LT 会を横に見つつ、k1low とコーディングをしていました。
k1low さんのコーディングスピード早い！

https://twitter.com/katzchum/status/1639500897352024065

お酒も入りつつペンライトを振りつつ、色々runn のインターフェースについてあーだこーだ考えていました。

■未来の開発者へ負債を残さないために取り組んだこと
https://speakerdeck.com/hayatokudou/wei-lai-nokai-fa-zhe-hefu-zhai-wocan-sanaitameniqu-rizu-ndakoto

DDD・レイヤードアーキテクチャ・OAS 活用をやられていて同じ様なアーキテクチャなので興味深く聞かせて頂きました。
こちらのツールも興味を引きました。

https://dredd.org/en/latest/


■クロージング

振返りを具体的な数値をまとめられて、しっかりやっていて Good でした。

https://twitter.com/katzchum/status/1639565696731615232


なんと本を頂きました！

■懇親会

本当に色々な方とお話させて頂きました！
Ask the Speaker で聞けなかった内容を質問させて頂いた所、以下のやり取りがあって教えて頂けたのが本当にありがたかったです。

https://twitter.com/katzchum/status/1639652732180238337


## まとめ

![まとめ](/images/articles/runn-developers-conference-in-phperkaigi2023/tweet2.png)
