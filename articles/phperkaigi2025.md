---
title: "PHPerKaigi 2025振り返り"
emoji: "🔦"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phperkaigi","conference","iwillblog"]
published: true
---

## PHPerKaigi 2025にスピーカーとして参加しました

今年も、[PHPerKaigi2025](https://phperkaigi.jp/2025/) に参加してきました！
去年に引き続き LT のスピーカーとしての参加となりました！

今回もバラエティ豊かなセッションがいくつもありました。
各セッションの感想については X のスレッド形式にまとめていますので、そちらをリンクしつつ纏めます。

私自身の登壇内容については [こちら](https://zenn.dev/katzumi/articles/phperkaigi2025#lt登壇) にまとめています。

## Day0

* [コードゴルファー道](https://fortee.jp/phperkaigi-2025/proposal/824ac541-67ad-4553-9eda-1267bc023229)
コードゴルフは本当に面白そうでした。
  * [X の実況スレッド](https://x.com/katzchum/status/1902984809170997400)

* [PHPerコードバトル 準々決勝 マッチ1](https://fortee.jp/phperkaigi-2025/proposal/cc66fef4-ebe4-480c-a92d-83f92008e51b)
ライブコーディングで競い合うという e-sports というのはあながち間違ってないと感じました！
  * [X の実況スレッド](https://x.com/katzchum/status/1902987042918965733)

* [The PHPer’s Guide to Daemon Crafting, Taming and Summoning](https://fortee.jp/phperkaigi-2025/proposal/d137c514-8ab2-45fc-9123-f9cd2733706d)
    @[speakerdeck](a4fddcca86644f8abd3abd46bc96c3ba)
nohup してアプリケーションを動かし続けるということを大昔やってたなーと思い出しました。デモが映らなくて大変残念でした。。
  * [X の実況スレッド](https://x.com/katzchum/status/1902994680062910870)

* [Alpine.js を活用した Laravel MPA フロントエンド最適化戦略](https://fortee.jp/phperkaigi-2025/proposal/fbcca06d-c654-4df9-b51a-8390b30e1ad7)
    @[speakerdeck](b122313f69534cd18e681ef186af2415)
    JQuery の代替の選択肢として Alpine.js で宣言的に使えて便利そうと思いました。  
    令和時代の JQuery というのはわかりやすい表現だと思いました。コンパイル不要というのもポイントが高いと感じました。
  * [X の実況スレッド](https://x.com/katzchum/status/1903009776495407447)

* [ドメインイベントを活用したPHPコードのリファクタリング](https://fortee.jp/phperkaigi-2025/proposal/5615c479-cc86-42db-b6b8-d3879e557a3e)
ドメインイベントの実装をコードレベルでサンプルを公開されているケースは少ない印象なので、参考になりました。  
イメージとしてクリーンアーキテクチャっぽいレイヤーを分けて実装がされて、テストしやすそうな印象を持ちました。
    @[speakerdeck](6a5e09c06baa49449b386ddd6d499c75)
  * [X の実況スレッド](https://x.com/katzchum/status/1903019508757062016)

* [PHPerコードバトル 決勝](https://fortee.jp/phperkaigi-2025/proposal/43dd50c8-cbf8-42be-80b4-124b8cb81b22)
takaram さん優勝おめでとうございます！
  * [X の実況スレッド](https://x.com/katzchum/status/1903028668026245461)


## Day1

午前中は体調が芳しく無く、ニコ生で視聴していました。

* [PHPによる"非"構造化プログラミング入門 -本当に熱いスパゲティコードを求めて](https://fortee.jp/phperkaigi-2025/proposal/81827a90-9586-4ca1-8367-858b20f2ebb9)
今回も色々示唆に富んだ面白いセッションでした。  
goto の嵐になるなーと思っていましたが、案の定でした。  
でも意外にラベル付とか頑張ると読めるなーというのが個人的な発見でした
    @[speakerdeck](0ed7381a11e3499da0311f71c0cc0bf2)
  * [X の実況スレッド](https://x.com/katzchum/status/1903261673676550482)

* [BCMathを高速化した一部始終をC言語でガチ目に解説する](https://fortee.jp/phperkaigi-2025/proposal/2303ec3c-9aa0-452d-b962-079272715857)
凄く丁寧に説明をして頂きましたが、最後は凄く難解で追いつけなかったですｗ
でもこれで高速化されて、その利益を我々が得られているというのは、ありがたい限りです。
    @[speakerdeck](ea4ac01af18745b089ade4007ce2016c)
  * [X の実況スレッド](https://x.com/katzchum/status/1903269865168789605)

大分体調が復活したので、会場へ移動。

* [大規模ふるさと納税サイトをPHP8化した時の苦労話](https://fortee.jp/phperkaigi-2025/proposal/6b95c65f-fca6-4c8d-925a-a018231e07e4)
生々しいお話が聞けて良かったです。  
  * [X の実況スレッド](https://x.com/katzchum/status/1903318735991288133)

* [「うわっ…うちのテスト、遅すぎ…？」 PHPUnit高速化テクニック](https://fortee.jp/phperkaigi-2025/proposal/5e50648e-3041-4f1b-ac64-320a3fdc5d45)
実直なテクニックが紹介されていて、学べるセッションでした。
setup-php の XDebug の無効化、OPCache の有効化は見直ししてみたいと思いました。
    @[speakerdeck](084b83c2002c4e32ac4f4c9825a30ac3)
  * [X の実況スレッド](https://x.com/katzchum/status/1903326532782960675)

* [プロダクトコードとOSSに学ぶ例外処理の選択肢 — キャッチするのか、投げっぱなしにするのか](https://fortee.jp/phperkaigi-2025/proposal/982d13a2-7840-4c9b-9ad9-3bade3e25606)
めちゃくちゃパワフルでしたーｗ  
ビジネス例外と技術例外とで分けてやるというのを具体的な OSS に当てはめて説明するというのがわかりやすかったです。
    @[speakerdeck](9204ee973d1e46518ae88cc654de0ac3)
  * [X の実況スレッド](https://x.com/katzchum/status/1903335516239802625)

* [ソフトウェア開発におけるインターフェイスという考え方](https://fortee.jp/phperkaigi-2025/proposal/69a88fbb-8796-4574-98af-f79e1ab3bca2)
去年の CLI の登壇から引き続きの話で今回は集大成的なセッションでした。ただ、より抽象度が高い概念的なものに触れることが出来てよかったです。  
OSS の開発時、大切にされているこだわりポイントが垣間見えた気がします。  
runn 開発者会議でよくインターフェースが決まらないんですよねーという発言をされていたので、そういうことねーという自分にとって学びが多いセッションでした。
    @[speakerdeck](2c66d2faae7943f98d0fefb2b781f710)
  * [X の実況スレッド](https://x.com/katzchum/status/1903344201565344119)

* [「PHPシンタックスコレクション〜ペチコレ〜」厨二病が好きそうな小難しいシンタックス編](https://fortee.jp/phperkaigi-2025/proposal/d3a053fa-7bd8-42c9-8053-2e6b7f6c7016)
凄く勢いがあって面白い LT でした。個人的に yield あるかな？と思って見ていました。
    @[speakerdeck](62e2dff1410044faa11507c334161cf3)
  * [X の実況スレッド](https://x.com/katzchum/status/1903368994494816368)

* [バックエンドエンジニアによるフロントエンドテスト拡充の具体的手法](https://fortee.jp/phperkaigi-2025/proposal/45edfee0-50d6-4f79-b93b-d3b444e4c795)
    @[speakerdeck](59d06d3fc840444d8d73d7811335f18f)
  * [X の実況スレッド](https://x.com/katzchum/status/1903374389242167794)

* [英語文法から学ぶ、クリーンな設計の秘訣](https://fortee.jp/phperkaigi-2025/proposal/b9e7bee5-795f-45f2-b147-b1ea79b5677b)
  * [X の実況スレッド](https://x.com/katzchum/status/1903372765257957553)

* [社内コードゴルフ大会を開催したら最高に楽しかった！！](https://fortee.jp/phperkaigi-2025/proposal/0fb87b9c-5a46-45f8-bb18-87ebd646bc94)
  * [X の実況スレッド](https://x.com/katzchum/status/1903375979994824948)

* [Today's Update](https://fortee.jp/phperkaigi-2025/proposal/ee433972-20fe-481f-b339-4f65bcf87020)
  * [X の実況スレッド](https://x.com/katzchum/status/1903381509047824523)

## Day2

* [モジュラーモノリスでスケーラブルなシステムを設計・開発する](https://fortee.jp/phperkaigi-2025/proposal/9d6ccf4b-6630-4c23-8bf6-948d24078504)
モジュラーモノリスの技術面・組織面の具体的なお話が聞けて良かったです。  
開発者体験も良くなったというのが、アーキテクチャの構成で下支えするというイメージをもっと聞きたいと思いました。
    @[speakerdeck](fa8b1e7c8d5949ecbed0b94c51f8b47f)
  * [X の実況スレッド](https://x.com/katzchum/status/1903619506557366387)

* [Terminal IDEの世界](https://fortee.jp/phperkaigi-2025/proposal/ba4329c1-2a34-41d9-adc9-38acf2d18507)
人生初の PHP コーディングがライブコーディングというのが大変おもしろかったです。  
  * [X の実況スレッド](https://x.com/katzchum/status/1903635666875126259)

* [アーキテクトと美学：美しさがシステムにもたらす秩序と未来](https://fortee.jp/phperkaigi-2025/proposal/65143400-54ab-4eb0-ad96-1044364de149)
審美眼を磨く為には、色々経験をしないといけないと思いますが、アートから取り入れるという気づきを得られました。  
言語化できる力を磨いていきたいと思いました。
  * [X の実況スレッド](https://x.com/katzchum/status/1903658299737944417)

* [PHP8.4におけるJITフレームワークIRと中間表現について理解を深める](https://fortee.jp/phperkaigi-2025/proposal/451aa71c-ba1a-4f2e-9704-de8622f5b54f)
JIT 完全に理解した！とはなかなかならないので今回も理解を深める機会になりました。  
ざっくり高次元な最適化が IR でできるようになったと理解しました。
    @[speakerdeck](3e62117eb54d411cb7c19ffdeb80f89b)
  * [X の実況スレッド](https://x.com/katzchum/status/1903667051656524184)

* [技術的負債を正しく理解し、正しく付き合う](https://fortee.jp/phperkaigi-2025/proposal/b00332b5-8965-4438-bcce-932c3b8f8f15)
実際のプロジェクトの会話のイメージを踏まえて凄く共感を持てるセッションでした。  
ビジネス上のリスクということを経営層にうまく説得していかないとなと思いました。
    @[speakerdeck](562c4d667b9e495789409458fdd377a5)
  * [X の実況スレッド](https://x.com/katzchum/status/1903680030624100422)

## LT登壇

「時間の許す限り yield の挙動を説明する」というタイトルで LT をしました！
プロポーザルは [こちら](https://fortee.jp/phperkaigi-2025/proposal/f918f32e-4f7b-4cab-89e6-f4e9cc4d0589)

@[docswell](https://www.docswell.com/s/katzumi/KN183E-yield-deep-dive)

プロポーザルのネタ振り通りしっかりと銅鑼を鳴らしましたｗ

> 時間が足りなくて銅鑼が鳴るか？それともネタが尽きるのが先か？そんなネタLTです。

今回のプロポーザルは、PHPerKaigi 2024 の LT 登壇した際に機材トラブルで内容をかなり端折って時間内に終わらせたという経験からになります。  
`逆に考えるんだ。「銅鑼は鳴らしちゃってもいいさ」と考えるんだ` という精神です。 

勢いでやり切りましたが、その後色々な皆さんに感想などを聞いてみたら、以下のようなお話を頂くことが出来ました。

* なかなかコードを読む時間がなかった
* 各クイズの制限時間に意味があったのか？
* 長くなってしまうと勢いもなくなってしまうので、LT として良かったのでは？  
* 興味を持ってくれたら後でスライド見てくれるのでは？
* yield でキーを重複して嵌まってしまうことありましたー
* yield の使い方難しいですよねー。フレームワーク側では使うけれどー
* PHPUnit の dataProvider で使うと、手続き的に配列を作れて便利ですよねー
* スライドであったあのページのヤツって、アレですよねー

ネタ LT でしたが、盛り込んだネタに気づいてくれた人も出てきて嬉しかったです。

あと、スライドでペンライトの写真を掲載したのですが、快く許可して頂いた [@kotomin_m](https://x.com/kotomin_m) さんありがとうございます！

https://x.com/kotomin_m/status/1903368944800661992

## runn開発者会議

https://x.com/katzchum/status/1903719801857716648

カンファレンスの廊下で、runn のご利用者から質問を受けての投稿でした。
マルチテナントなシステムで DB 環境を構築する際に runn を使って試された際に、runbook 実行後の後処理を追加したいとのことでした。  

今回は [@k1low](https://x.com/k1LoW) さんとなかなかタイミングが合わず混み合ったお話が出来なくて残念でした。  
@k1low さんの登壇後の Ask the speaker でバージョン番号もインターフェースですか？とご質問させて頂いて ver1.0.0 どうしましょうｗというお話をさせて頂いていました。

## 感想

PHPerKaigi は今年で 3 回目になりますが、新しい取り組みや新しい人がどんどん入ってくる楽しいカンファレンスだと感じています。  
また色々な面でクオリティが高いと毎回関心させられます。  
特にスタッフの方が、朝笑顔で迎え入れてくれたのが、印象的でスタッフの方も楽しまれてやられていて、本当に素晴らしいと感じました。

