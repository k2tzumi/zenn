---
title: "PHPerKaigi 2024振り返り"
emoji: "🐘"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phperkaigi","conference","iwillblog"]
published: true
---

## お詫び

3 ヶ月も経過してしまっていますが、[PHPerKaigi202４](https://phperkaigi.jp/2024/) の参加記事となります。
今年は本業の事業ドメイン的に忙しく、また 3 月はイベントも重なってしまったので完全に出遅れてしました。

以下で印象に残ったセッションや出来事をピックアップしてまとめます。

## Day1

■Command-line interface tool design
https://speakerdeck.com/k1low/phperkaigi-2024

色々な CLI ツールを作成されている @k1low さんならではの発表でした。  
CLI ツールのオプションだけでなく、Unit 哲学的な CLI としての使い方（I/F）をどうデザインするか？という点が、大変学びがありました。  
runn でもオプションが多数存在するので、今回の話が設計ガイドライン的な考え方として腹落ちしました。

■Laravel OpenAPI による "辛くない" スキーマ駆動開発
https://speakerdeck.com/kentaroutakeda/laravel-openapiniyoru-xin-kunai-sukimaqu-dong-kai-fa

当日は、リアルタイムで登壇は聞けなかったのですが、個人的に楽しみだったセッションでした。  
YouTube で公開はありがたいです。  
自身もスキーマ駆動開発に取り組んでいて、取り組みや課題とその考え方についてすごくわかる～という内容でした。  
今度今のお仕事で取り組んでいる内容を発表したいと感じました。

■どうやって Web サービスのページ表示速度を 1/3 にしたか
https://speakerdeck.com/pinkumohikan/how-to-reduce-display-speed

推測するな、計測せよということを肝に着実に改善させていて、凄い良いお話でした。

## Day2

■Web アプリケーションの効率を再定義する BEAR.Sunday の分散キャッシングフレームワーク  
https://speakerdeck.com/koriym/ibentoqu-dong-kontentu

今回のイベントで、一番興味深かったセッションでした。
初めて知ったフレームワークのですが、独自の世界観と思想がある惹きつけられる内容でした。

■phpunit/php-code-coverage って何をしてるんだ  
https://speakerdeck.com/o0h/phperkaigi-2024

自分が LT する内容と被っているなーと思っていたら、事前公開された [補足スライド](https://speakerdeck.com/o0h/phperkaigi-2024-omake) が素敵すぎました。  
LT 内容の補完する内容として大変ありがたかったです。

■API テストでもカバレッジ測定したい！   
https://www.docswell.com/s/katzumi/ZW17J8-how-to-measure-api-test-coverage

runn と連携してカバレッジを取得させるという内容で発表しようとしました。  
LT でハプニングが発生し（なぜかスライドの一部が表示されない）、完全にパニックしてしまいました。 
かなり端折って時間内に終わらせましたが、不完全燃焼な結果となりました。 

## runn開発者会議

k1low さんといつもの廊下で会話して議論していきました。

https://twitter.com/k1LoW/status/1765956514077163932

ちょうど 1 年前から runn 開発者会議が始まったんだなーと感慨深いものがありました。

## まとめ

登壇者へのフィードバックももらったのですが、LT での感想でも「ナイスリカバリーでした！」という内容を頂き PHP の Tech イベントの温かみを実感しました。