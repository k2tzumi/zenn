---
title: "PHP Conference Japan 2023参加レポート"
emoji: "♨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phpcon","conference","iwillblog"]
published: true
---

## はじめに

ブログを書くまでがカンファレンスですということで PHP Conference Japan 2023 の参加レポートです。  

https://phpcon.php.gr.jp/2023/


今年は 3 月の PHPerKaigi ^[PHPerKaigiの感想記事は [こちら](https://zenn.dev/katzumi/articles/runn-developers-conference-in-phperkaigi2023)] に始まり、カンファレンス熱が上がったのか？続けて

* PHP カンファレンス福岡 ^[PHPカンファレンス福岡の感想記事は[こちら](https://zenn.dev/katzumi/articles/phpcon-fukuoka-2023-talk-impression)]
* PHP カンファレンス沖縄 ^[PHPカンファレンス沖縄の感想記事は[こちら](https://zenn.dev/katzumi/articles/phpcon-okinawa-2023)]

へと地方遠征して登壇を行うなどしていました。  
今回は電車移動圏内での一般参加で、のんびり参加できて良かったです。

## 前夜祭

福岡では前夜祭から参加して、色々な方と交友を広げることができて遠征組として大変ありがたいと感じました。

今回は [@meihei](https://zenn.dev/meihei) さんが Nice な会を企画してくれました。本当にありがたい！ ^[イベントページの写真は下見されてきた際のものらしいです]

https://connpass.com/event/296001/

沖縄のカンファレンスの [後日談](https://zenn.dev/katzumi/articles/phpcon-okinawa-2023#%E5%BE%8C%E6%97%A5%E8%AB%87) でやりとりさせて頂いた [@77web](https://zenn.dev/77web) さんと Doctrine のお話をさせて頂く機会ができたので大変良かったです。

@[speakerdeck](bef498937c214803883a46a2826141c8)

毎度 PHP 成分が少なくて申し訳ないのですが、LT にも参加してきました。

温泉の黒湯も堪能できたのもあり、すごくほっこりしました。

## 当日

今回は Track6 まであり、身1つではとても回りきれませんでした。
参加したものでいくつかピックアップして感想をまとめたいと思います。

:::message
【宣伝】
Track が多いので [別記事](https://zenn.dev/katzumi/articles/php-conference-japan-2023-slides-link)にセッションとスライドをまとめています
:::

### [Laravelのコードとわたしたちのコードとの距離を保つ](https://speakerdeck.com/blue_goheimochi/phpcon2023)

クリーンアーキテクチャやってたりすると、フレームワークへの依存はどこまでしてしまって良いんだっけ？となりがちなので、気になっていたセッションでした。
弊害として Eloquent の指摘があり、やっぱりなーという気持ちになりました。 
インターフェースを利用してドメインロジックを Clean にするのは大変わかりやすかったです。  
Doctrine 推しをされていたのが印象的でした w

### [PHP8.2から見る、２つの配列](https://speakerdeck.com/meihei3/php-conference-japan-2023)

配列が月刊 PHP カンファレンスなのに吹き出しましたｗ
php-src をみんな読んでいて偉い！となりました。 
Packed Array という用語や、実装が異なるというのは初めてしりました。  
セッションで聞き漏らしてしまっているかもですが、Packed Array を途中から HashTable として扱うとどうなるんだろう？という疑問が残りました。

### [25分で理解する！Symfonyの魅力とその実践的活用法](https://speakerdeck.com/ippey/25fen-deli-jie-suru-symfonynomei-li-tosonoshi-jian-de-huo-yong-fa)

Symfony 理解したいです！
Laravel にも一部 Symfony のコンポーネントが組み込まれていたり、必要なときに必要なだけ使えるのは Symfony の良さだと思うので、部分部分でお試ししたいなーという気持ちにさせられる良いセッションでした。 
セッションで紹介されていた Symfony の機能で良さそうと感じた所を以下にまとめます

* Attributes を解釈して、DB のスキーマ情報から自動で migration 作成されるのは sqldef っぽくて良い
* Attributes を活用した開発スタイルは、今やっているプロダクトと方向性が似ているので相性が良さそう
無意識に疎結合に近づく開発体験というのは凄い良い考え方だなーと。  
* ValueResolver というのも、リクエストとエンティティの関連を疎結合に定義できて良さそう

> API Platformを使えばAttributesを足すだけでAPIの作成が可能

こちらの内容はまたどこかで聞きたい内容でした。

ちょうぜつ補足を頂いたので大変ありがたかったです。

https://twitter.com/ippey_s/status/1711227274631270725


### [PHPerにとってのWebAssemblyの可能性](https://speakerdeck.com/nagano/phpernitotutenowebassemblynoke-neng-xing)

https://php-play.dev にお世話になっています。
Wasm ぜんぜん理解してないですが、golang で build できるということと、GAS でも使えるっぽい記事をみて何か出来ないかな〜と気になっています。  

X のポストに回答頂いてありがたかったです。

https://twitter.com/glassmonekey/status/1710906127679213844

https://twitter.com/glassmonekey/status/1710905713223172413


以前、GAS で LGTM 画像を作ろうとして外部 API（Cloudinary）にお世話になった経験があります。
https://zenn.dev/katzumi/articles/0e9c0a49258afc

GAS で標準の npm ライブラリが使えなかったりするので、Wasm を使ってなんとか出来ないかな〜と考えています。  
時間があれば、試してみたいと思います。

### [Symfony+Doctrine ORMで始める安全なモジュラモノリス](https://speakerdeck.com/77web/symfony-plus-doctrine-ormdeshi-meruan-quan-namoziyuramonorisu)


今の開発しているサービスがモジュラモノリスっぽく開発しているので、すごく気なるセッションでした。  
Laravel の Eloquent は ActiveRecord パターンでクリーンアーキテクチャとは相性が良く、逆に相性が良さそうな DataMapper な Doctrine にすごく興味がありました。

Laravel でモジュラモノリスを実践して苦労しているわけではなかったのですが、

* たぶんほぼ Eloquent が悪いので laravel-doctrine を使って Laravel の中で Doctrine を使ってほしい
* 密結合指向の Laravel ではなく疎結合指向の Symfony に移行することも選択肢に入れてみてほしい

というのは刺さりまくったセッションでした。
あと [#symfony-ja](https://phpusers-ja.slack.com/archives/C90LYS1QF) チャネルがあるのを知れてよかったです。

## 月刊PHPカンファレンスについて

https://twitter.com/kotomin_m/status/1710941870497681424

クロージングでなかなか凄い発表がありましたｗ
来年もカンファレンスがあつそうですｗ
個人的には来年の 5 月まで業務が忙しく、遠征は難しいと思いますが、スライド等で楽しみたいと思います。  
あと福岡でお会いした [びきニキ](https://twitter.com/BkNkbot)さんが実行委員長をやられると聞いてびっくりしたのと、またパワーを貰いに福岡へ遠征したいと思いました。

https://twitter.com/BkNkbot/status/1711017449485898028


## 懇親会

アンカンファレンス向けにまとめていたスライドがあったのですが、アンカンファレンスの登壇表明のホワイトボードが見つからずでした。。
最終的に見つけることは出来ましたが、時すでにお寿司だったので懇親会 LT 向けに振り替えました。
@[speakerdeck](7b02f6cf8bc24b1db8bdd8f5967c40bb)

他の LT の方が強すぎて LT を出すか凄く悩みましたｗ
ただ LT の後にお声がけして頂いた方が出てきてやって良かった！という感じになりました。

## まとめ

久しぶりの遠征でないカンファレンスでしたが、前夜祭と後夜祭 ^[自分は参加しませんでしたが、[PHP Lovers Meetup](https://php-lovers-meetup.connpass.com/event/297161/)があった様です] も含めて参加すると満足感が高いと実感できました。
遠征組と開催地組の交流の場として、大変貴重だと思います。
企画して運営してくださった皆様に感謝を伝えたいです！ありがとうございました！


懇親会や 2 次会も楽しいのですが、遠征組と一緒に遅くまでワイワイする場合には近場に宿を取ると良いという学びを得ましたｗ

https://twitter.com/katzchum/status/1711347272171323746

