---
title: "runn開発者会議 in PHPerKaigi 2023"
emoji: "🐘"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phperkaigi","conference","iwillblog"]
published: true
---

## はじめに

ブログを書くまでがカンファレンスですということで [PHPerKaigi2023](https://phperkaigi.jp/2023/) の参加の体験記事的なやつです。  
なにげにオフラインでの参加は初でした。  

初めて参加しようとなったきっかけは以下のやり取りでありました

https://twitter.com/k1LoW/status/1635622359808999425

![やりとり](/images/articles/runn-developers-conference-in-phperkaigi2023/tweet.png)

記事のタイトルにあるrunnというツールの開発者である [@k1low](https://twitter.com/k1low) さんと初顔合わせの実績を解除することができました！  
runnって何？という方は以下の記事を参照ください。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

以下に印象に残ったセッションや出来事をピックアップしてまとめます。

## Day0

お仕事だったのでオンラインで途中参加

■PHPUnit 10 概論
https://speakerdeck.com/cocoeyes02/introduction-of-phpunit-10

ホットな話題でありがたかったです。
以前使えていたいくつかの関数が使えなくなっているらしいです。
今携わっているプロジェクトで適用済みだけれどdataProvider以外はそれほど苦労しなかったけれど、使い込んでいるプロジェクトでは対応が大変そう。

■アンドキュメンテッド「ちょうぜつソフトウェア設計入門」
https://speakerdeck.com/tanakahisateru/andokiyumentetudo-tiyouzetusohutouea-she-ji-ru-men-obuziekutozhi-xiang-niding-yi-hanai-noka

個人的にツボにはまったセッションでした。
ソフトウェア開発の歴史をまとめていくと色々見えてくるものがあるのでは？という良い気付きになりました。  
技術の進化の歴史は振り子ではなく螺旋である ^[https://speakerdeck.com/twada/understanding-the-spiral-of-technologies] という話があります。その螺旋の差分を抑えるには歴史から学ぶ必要があると感じました。


## Day1

■PHPで学ぶ "Cacheの距離" の話
https://speakerdeck.com/hanhan1978/study-cache-with-php

一番最初はキャッシュのお話。  
キャッシュの種類が２つあり、そキャッシュする目的と手段が異なってくることを丁寧に説明されていました。
PHPのシリアライザーがバージョンによって挙動が変わらないかなという疑問がありましたが、最初のセッションで質問するハードルが高くて聞けませんでした。

■Win Testing Trophy Easily / テスティングトロフィーを獲得する
https://speakerdeck.com/k1low/phperkaigi-2023

テスティングトロフィーという用語を初めて知りました。
アーキテクチャだけ決まっていて、テスト戦略を考えた結果APIシナリオテストに辿り着いという話は、runnが生み出される重要な動機になっているので大変興味深く聞きました。  
APIとDBのスキーマは変化が少ないという着眼点し、長期的に維持し易いテストスイートを目指したという話は、自分が実際にrunnを使っていてコスパが良いと実感できているので、狙い通りで撃ち抜かれたと思いましたｗ  
すべてのバックエンドエンジニアには是非runnを使ってもらいたいと感じています。  
最近はSPAなプロダクトが多くてフロントも含めてE2Eテストしたいと感じている方も多いかと思います。  
ただE2Eテストの導入のハードルの高さや、始めたけれどテストがすぐ壊れて維持できず断念した方も多いのではないでしょうか。私も実際に検討しましたが諦めてしまっていました。  
runnを使ったAPIシナリオテストであれば、学習コストや導入手間がほとんどなくCIまで組み込むことが可能です。  
フロントの実装が完了していない状態でAPIテストをPostmanやcurlコマンドなどで手動で確認したことがある場合は、ぜひrunnでAPIを呼び出してみてください。直ぐに気に入ると思います。  
作ったシナリオファイルは優秀な動くドキュメントになるので、フロントチームが独立している場合はドキュメント代わりに是非書いてみることをオススメします。  
おまけのスライドでも触れられていますが、deploy直後のリグレッションテストにそのまま使えるのでポチポチテストを代替させることも出来ます。一粒で3度ぐらい美味しいですw

■QA
Q. 一つのシナリオファイルで複数データパターンを扱うことは出来ますか？
A. jsonファイルを読み出してデータパターン分繰り返し実行できます。
このデータ駆動テストの機能拡張を行ってくれたコントリビューターが本日会場に来てくれていると思います（ニヤリ

https://twitter.com/katzchum/status/1639098721744080897

はい。私ですｗ

■Ask the Speaker
先に3名ぐらいお話をされていて、落ち着いた頃を見計らって初めてのご挨拶をさせていただきました。  

軽く感想や雑談をしてからrunn談義がスタートしました。  
しばらくしてからk1lowさんから以下のIssueについて話をしたいと頂きました。

https://github.com/k1LoW/runn/issues/397

結構以前に作成したIssueだったので、実現したいこと・具体的なユースケースについて説明をしました。  
普段はIssueやPRでやり取りしていましたが、やはりダイレクトにネイティブ言語でやりとりできるといいですね。

■防衛的 PHP: 多様性を生き抜くための PHP 入門 
https://speakerdeck.com/siketyan/defensive-php
PHPStormがCIで動くんですか！？
良いことを聞いたので試したいと思います。


■k1lowさんからのFB

https://twitter.com/k1LoW/status/1639128685856067584

Ask the Speakerで話をしていた件につい早速フィードバックいただきました。


■安全にプロセスを停止するためにシグナル制御を学ぼう！ 
https://speakerdeck.com/yamamotohiroya/an-quan-nihurosesuwoting-zhi-surutamenisikunaruzhi-yu-woxue-hou
このセッションを聞いて、Laravel Queue workerを安全に止める方法が気になって調べてみましたが、オフィシャルで停止コマンドが用意されていなくて少しびっくりしています。  
安全に止める方法をご存知の方は教えていただけると嬉しいです。


■Rector ではじめる "運用を止めない" PHP アップグレード
https://fortee.jp/phperkaigi-2023/proposal/d3084a8f-89b5-48d6-8a81-59c460b3b3e0
スライド見つからず

Rectorを最近導入していたので興味津々でした。  
RectorをCIに組み込んだことで、非互換のコードが混入しないようになったという話になるほどーと感心しました。 
目的が達成されたらRectorを削除したというのも良いと思いました。


■LT大会（6本）

みんな勢いがあって凄いw

■推測するな、計測せよ New Relic × Laravel 実践
https://speakerdeck.com/mpyw/tui-ce-suruna-ji-ce-seyo-new-relic-x-laravel-shi-jian
オンライン登壇なのに会場で本人解説付きで面白かったですw

■池袋Night

https://twitter.com/k1LoW/status/1639205573316206598

本当に色々なお話をさせていただきました。  
貴重な話で内容が濃く、Podcastにすると面白そうと感じました。

## Day2

午前中は用事があり午後から参加

■k1lowさんのフットワークの軽さ

https://twitter.com/k1LoW/status/1639453320908779522

■アンカンファレンス

無限LTが開始されていたので覗いていました。  
現同僚と元同僚がそれぞれLTで発表していた。


■Attributeを極める
https://fortee.jp/phperkaigi-2023/proposal/86a02001-4409-49c9-9fbe-4db251c96cb0

Attributeネタを継続的に発信して頂けているの有り難いです。  


■runn開発者会議 2日目

https://twitter.com/k1LoW/status/1639543221553606657

ホワイトボードも使えるのもリアルならではです。

■LT大会2日目

LT会を横に見つつ、k1lowとコーディングをしていました。
k1lowさんのコーディングスピード早い！

https://twitter.com/katzchum/status/1639500897352024065

お酒も入りつつペンライトを振りつつ、色々runnのインターフェースについてあーだこーだ考えていました。

■未来の開発者へ負債を残さないために取り組んだこと
https://speakerdeck.com/hayatokudou/wei-lai-nokai-fa-zhe-hefu-zhai-wocan-sanaitameniqu-rizu-ndakoto

DDD・レイヤードアーキテクチャ・OAS活用をやられていて同じ様なアーキテクチャなので興味深く聞かせて頂きました。
こちらのツールも興味を引きました。

https://dredd.org/en/latest/


■クロージング

振返りを具体的な数値をまとめられて、しっかりやっていてGoodでした。

https://twitter.com/katzchum/status/1639565696731615232


なんと本を頂きました！

■懇親会

本当に色々な方とお話させて頂きました！
Ask the Speakerで聞けなかった内容を質問させて頂いた所、以下のやり取りがあって教えて頂けたのが本当にありがたかったです。

https://twitter.com/katzchum/status/1639652732180238337


## まとめ

![まとめ](/images/articles/runn-developers-conference-in-phperkaigi2023/tweet2.png)
