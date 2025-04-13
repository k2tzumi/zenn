---
title: "ぺちおだ2025参戦してきました"
emoji: "🍋"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phpcon","conference","iwillblog"]
published: true
---

## ぺちおだ2025に参戦してきました

今年も、[ぺちおだ2025](https://phpcon-odawara.jp/2025/) に参加してきました！
小田原には [紅白ぺぱ合戦](https://connpass.com/event/329428/) 以来で 3 回目のイベント参加となりました。

今回も前夜祭から 2Days で参加して最終日は小田原観光してきました。
各セッションの感想については X のスレッド形式にまとめていますので、そちらをリンクしつつ纏めます。

では行ってみましょ〜

「エーッサ エーッサ エッサホイサッサッ♪」

https://x.com/katzchum/status/1910594431906177357

## [ぺちおだ2025 zenyasai（前夜祭）](https://phpcon-odawara.connpass.com/event/338461/)

* [だから僕は（クラス）設計を辞めた](https://fortee.jp/phpconodawara-2025/proposal/59bfacd6-ec67-4758-9148-8f4403cfcb12)  
  @[docswell](https://www.docswell.com/s/stwile/KP2318-2025-04-11-breaking-up-with-class-design)
フルスタックフレームワークのレールにのっかって、クラス設計を遅延的に行うという様な内容でした。  
複数人で不揃いなクラス設計が乱立するよりもトランザクションスクリプトの方が、認知負荷が低いことがあったりします。
捨てやすい状態を保つ取り組みだと感じました。

* [サービスクラスのありがたみを発見したときの思い出](https://fortee.jp/phpconodawara-2025/proposal/118409a3-759c-437b-9133-7fa126e0683e)
サービスクラス=ユースケース=サービスレイヤーと多分同じことを言っていて初学者が混乱しがちなので、もっと力を抜いて感じて理解するだけで良いよーという勇気を貰う内容でした
    @[speakerdeck](a22830d9ec8048c2a33d13dd1c03f665)
  * [X の実況スレッド](https://x.com/katzchum/status/1910630278680514612)


* [社内コードゴルフ大会を開催したら最高に楽しかった！！](https://fortee.jp/phpconodawara-2025/proposal/470e51a1-e6ea-497e-b584-6e56f39d48a8)
コードゴルフの環境を作って、継続的に大会を開発されていた話でした。WASM 版も開発されているみたいで今後も期待です！
  * [X の実況スレッド](https://x.com/katzchum/status/1910635532931199225)

* LT② tyamahori さん
Devbox はいいという内容でした。NIX 教えて欲しいと思いました。
  * [X の実況スレッド](https://x.com/katzchum/status/1910637393352548450)

* LT③ kaga さん
他のコミュニティに参加されている人と共通のネットワークがあると幅が広がるというのは、ペパ合戦や gocon に参加していて実際に感じていることなので、推しを増やしていこうと思いました。
    @[speakerdeck](10749e45bdb4455080eddff912694543)
  * [X の実況スレッド](https://x.com/katzchum/status/1910638689748943033)

* LT④ tsumetai-udon さん
SSRF（Server-Side Request Forgery）を実演する LT でした。過去に携わっていたサービスで脆弱性診断で引っかかったやつだったので、こんなにヤバかったんだ！という感じでした。
  * [X の実況スレッド](https://x.com/katzchum/status/1910640279692206358)

## 宮小路散策ナイト

宮小路にある [スナック無駄](https://www.instagram.com/snack_muda/) で楽しい話をいっぱいしてきました。
サウナの奥深い話を聞けて興味深かったです。

https://x.com/katzchum/status/1910693112328183854

こんなカッコいいステージがあり、マスターの生演奏もついてきます！

![スナック無駄のステージ](/images/articles/phpcon-odawara-2025/snack_muda.png)

マスターのエモエモな note もおすすめなので是非！

https://note.com/phpcon_odawara/n/n2e52fd8b723f


## PHPカンファレンス小田原2025


* [OSSコントリビュートをphp-srcメンテナの立場から語る](https://fortee.jp/phpconodawara-2025/proposal/783ac013-b506-4fa0-b6c2-62e99a2a95f7)
新しいコントリビューターが増えるのは凄く嬉しくてというのは、凄く共感しました。海外 OSS 且つ大規模リポジトリならではの気づきを聞けて良かったです。
    @[speakerdeck](b46b947b6c85432380248716c5df9c61)
  * [X の実況スレッド](https://x.com/katzchum/status/1910865569508995515)

* [PHP 8.x 時代のクラス設計（property promotion から property hooks まで）](https://fortee.jp/phpconodawara-2025/proposal/56be018e-75cf-41a7-88a2-cc15e92d741e)
どんどん PHP も良くなってきたなーというのを改めて実感させられる内容でした。  
設計にどうフィードバックしていくのが良いか？というのもよくまとまっていました。  
property hooks はまだ試せてないので、もっと聞きたい！と思いました。
  * [X の実況スレッド](https://x.com/katzchum/status/1910875743929983166)

* [PHPと旅する OSI 7階層](https://fortee.jp/phpconodawara-2025/proposal/db76a0c7-a721-4e3b-9245-72d432105b19)
各 Layer をダンジョン攻略の様に、PHP で実装していくのが凄く面白いトーク内容でした。  
やっぱり深層部（低レイヤー）は高難易度なんですねーという感想でした。  
各プロトコルをおしゃべりできる用にするというのは、その言葉を理解していかないといけないので学びが多そうと感じました。
  * [X の実況スレッド](https://x.com/katzchum/status/1910884659204137290)

* [PHPバージョンアップから始めるOSSコントリビュート](https://fortee.jp/phpconodawara-2025/proposal/888dd742-0295-4982-b267-8f1c0072c38b)
PHPUnit 設定で　displayDetailsOnPhpunitDeprecations="true"　を有効にするというテクニックを試してみたいと思いました。　 
PHP8.4 対応の Rector のルールをそういえば入れてなかったと思い出しました。
    @[speakerdeck](229a4fd11cc74415886e7352cbbe2f03)
  * [X の実況スレッド](https://x.com/katzchum/status/1910889655572066486)

* [古き良きLaravelのシステムは関数型スタイルでリファクタできるのか](https://fortee.jp/phpconodawara-2025/proposal/dcf4e085-d653-4d51-9f87-574a3a79f7e2)
状態遷移が増える事に nullable なカラムが増える DB・モデル設計という話で、しかもそれが 120 カラムもあるとわかると一気に会場がざわつきましたｗ　 
凄い堅実なステップを着実に刻まれているのが、好感をもてました。 
個人的にドメインイベントを明示的に定義して、事実を確実に残す様にしていったらどういうステップを踏んでいくのがいいだろうか？という所を考えながら見ていました。 
Eloquent のモデルイベントをドメインに切り出しとあったので、結果ドメインイベントができたのでは？と感じました。
    @[speakerdeck](370920ae44cb45bf97dde8d29440ab32)
  * [X の実況スレッド](https://x.com/katzchum/status/1910894733825827047)

* おだわランチ
シャッフルランチで行ってきました！

https://note.com/phpcon_odawara/n/n8fb5d67e604c

去年の 2 次会でいった [洋風居酒屋 La palourde（ラ・パルールド）](https://lapalowude.com/)さんに行ってきました。  

![ラ・パルールドのアジフライ定食](/images/articles/phpcon-odawara-2025/lapalowude.png)

店員さんがオーダーを聞いてくれなくて、少しドキドキしましたが専用のメニューを用意してくれていたみたいでした。  
名札プレートを見せるだけで、料理が出てくる凄いホスピタリティでした！

* [「影響が少ない」を自分の目でみてみる](https://fortee.jp/phpconodawara-2025/proposal/10386b1c-679e-4a3b-b64d-9f9a0b4c98f7)
ベテランの視点を難しい言葉を使わずに丁寧に言語化するという難しい課題に取り組まれていたトークでした。 
影響スケッチというのを始めてしったので、興味深くみていました。  
処理を図式化する際にデータフローと処理フローを混ぜて書いてしまって、結果ごちゃごちゃになってしまうので毎回悩みます。  
データの input と output を明確にしていきつつモデリングをごちゃごちゃするのが個人的には好きなのですが、どんどんモデルが大きくなってしまうのもあるので難しいと感じています。  
その中で、今回の「影響の距離を縮める」というのと「余分な情報をもたせない」というのを意識すると良いというのは凄く気付かされた気がしました。
    @[speakerdeck](249204c5841642409ce545bf3dd533a3)
  * [X の実況スレッド](https://x.com/katzchum/status/1910921266246341029)

* [定義ジャンプ入門](https://fortee.jp/phpconodawara-2025/proposal/6d853d31-19ed-4e75-b7fb-841b9490d9a8)
[google スライド](https://docs.google.com/presentation/d/1HkiYZNKf6YBBOPNsHcvuOq2IKp1fw0L40j_T7RhrKZE)  
LSP なお話でした。　 
AI を使うと 1 ドルかかってコードジャンプできるというのは、やっぱり出来るんだーという感じでした。  
Ask the Speaker で最近気になっていた MCP についても聞いてました。　  
runn で LCP 対応したいよねーという話が上がっていたけれど、最近流行りの MCP サーバー作ったら開発者体験が上がるかなーという妄想をお話させて頂きました。 
あと yaml はアレなので別のフォーマット（toml）だと嬉しいというご意見も頂きました。
  * [X の実況スレッド](https://x.com/katzchum/status/1910929919204614306)

* [New RelicのAPMを活用したECサービスにおけるメール遅延解消の舞台裏](https://fortee.jp/phpconodawara-2025/proposal/fa227b73-7c9e-4c79-a484-ff3309cb0ccf)
昔カートシステムを開発していたので、メール配信のトラブルに伴うあれこれはあるあるーと思ってみていました。　　
APM というものが、なかった時代の経験と勘に頼っていた感じなのでちゃんと計測するの本当に大事だと感じました。
    @[speakerdeck](84bdf3cd29f0403084df805304644bb1)
  * [X の実況スレッド](https://x.com/katzchum/status/1910939233550020707)

* [小さな開発会社が受託開発に力を入れる理由](https://fortee.jp/phpconodawara-2025/proposal/4791c482-607c-4092-9972-592c84bdd63a)
  * [X の実況スレッド](https://x.com/katzchum/status/1910949681150218496)

* [PhpStorm超絶技巧5分集中講座2025.4](https://fortee.jp/phpconodawara-2025/proposal/9dcb6cb4-866b-4b14-8540-bde2d7264568)
  * [X の実況スレッド](https://x.com/katzchum/status/1910951224196948161)

* [ちいさくPHPUnitをつくってみる](https://fortee.jp/phpconodawara-2025/proposal/14cd227c-f8d6-4eb5-a05e-bd172bf4cefe)
  * [X の実況スレッド](https://x.com/katzchum/status/1910955056318590982)

* [これ【<?php】がないと始まらない！PHPインタプリタ](https://fortee.jp/phpconodawara-2025/proposal/928d4226-4c2c-4cc0-b1ea-372442d9d86d)
  * [X の実況スレッド](https://x.com/katzchum/status/1910956295424815132)

* [2025年春に見直したい、リソース最適化の基本](https://fortee.jp/phpconodawara-2025/proposal/e97298ff-3d40-4bc0-8e5e-1fab5c2480ab)
    @[speakerdeck](b410522bd7884927ab85e317f4e46777)
  * [X の実況スレッド](https://x.com/katzchum/status/1910957510154215462)

* [100行で書けるPSR-11](https://fortee.jp/phpconodawara-2025/proposal/b54135a6-4b49-4c57-b742-c787aa238c2d)
    @[speakerdeck](134e5722d7be4c5a879de54baf1180c4)
  * [X の実況スレッド](https://x.com/katzchum/status/1910958958418051266)

* [小田原で　みんなで一句　詠みたいな](https://fortee.jp/phpconodawara-2025/proposal/87a6bd7b-56b8-41e2-b53f-1cf21211a400)
    @[speakerdeck](e40690970b314a59bd6b887e6940a9ae)
  * [X の実況スレッド](https://x.com/katzchum/status/1910960321193275850)

## ぺちおだ大合戦

https://x.com/sogaoh/status/1910968682651734272

三名人チームで合戦を戦い抜きました！
懇親会ですてにゃんさんに聞きましたが、凄い手が掛かっているゲームでした。
かなり盛り上がり、チームの連帯感も生まれて良かったです！
英単語知らなさすぎ問題がありましたｗ

## 懇親会

万葉の湯での懇親会で、会が終わったらそのまま温泉に入れるという極楽仕様でした。
料理。特にお魚が美味しく


## おだわらメシ＆観光

当日の朝食なににするか悩んだのですが、ホテルの近くのキッチンかぶらさんが土曜日でも 7:00〜オープンしていたので飛び込みで入りました。

![キッチンかぶら](/images/articles/phpcon-odawara-2025/kitchinkabura.png)
![ビュッフェ](/images/articles/phpcon-odawara-2025/buffet.png)

朝食に選んだキッチンかぶらは少し待ちましたが、正解でした。
メニューも豊富で、ひとつひとつが美味しくありがてぇ〜となりました。

後夜祭は参加せずに小田原観光をしてきました。
生憎の雨だったのですが、グルメ中心に回ってきました。

![海への扉](/images/articles/phpcon-odawara-2025/gate-to-the-ocean.png)
![相模湾を覗く](/images/articles/phpcon-odawara-2025/look-into-sagami-bay.png)

まずは名所を調べていたら出てきた海への扉というスポットに行ってきました。  
雨でなければ、青い海が見れるようです。

小腹が空いたのでオープンしたパン屋さんを見つけて頂きました。

![デニッシュ](/images/articles/phpcon-odawara-2025/danish.png)

下調べしていたお店が朝食をやめてしまっていたみたいなので、近くにあったお魚屋さんへいきました。

![かしわ水産](/images/articles/phpcon-odawara-2025/kashiwasuisan.png)
![刺身定食](/images/articles/phpcon-odawara-2025/sashimi-set-menu.png)

少し値が張りますが、あさイチから食べられるメリットと、お値段以上の美味しさに大満足でした。  
朝の食事難民が多そうな雰囲気なので、今度は朝ご飯紹介して貰えたらと思いました。

食べた後に時間があったので、そのまま足を伸ばして箱根ビールを飲みにいきました。

https://x.com/katzchum/status/1911230823673811026

昼はランチマップでも紹介されている味噌ラーメンを食べにいきました。

https://x.com/katzchum/status/1911258765468614896

あとはお土産を買って帰宅しました。
お土産は、買う場所によって大分値段が違うなーと感じるなどしましたｗ

