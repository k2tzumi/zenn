---
title: "PHPカンファレンス福岡2023で登壇してきました（Jun 22-25）"
emoji: "🍲"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phpconfuk","conference","iwillblog","runn"]
published: true
---

## PHPカンファレンス福岡2023で登壇してきました

カンファレンスの遠征初参加＆初登壇してきましたー  

https://phpcon.fukuoka.jp/2023/

CfP の段階から熱量が凄そうと感じていました。

https://twitter.com/katzchum/status/1645438807423205380

長くなるので最初に一言で感想を書くと `最&Co.` でした。  
カンファレンスから１週間たちましたが、まだ余韻が残っている中、本記事を書いています。４日間はかなり濃密でした。  
思った以上に熱量が高く、色々な方々とお話をさせて頂いて本当に良い経験をさせて頂きました。　　
いつもは一般参加の聞き専門勢でしたが、今回始めて飛び込み LT や登壇をさせて頂いて他の登壇者や運営の方々との距離が大分近く感じることができました。  
少し長くなりますが、時系列に参加したイベントや出来事をまとめていきます。　　
登壇内容だけを見たいよ！という方は [こちら](#登壇〜ask-the-speaker) からお願いします。

## 前々日

### 全然野菜

福岡には前々日の午後に現地入して、夕方は全然野菜に参加してきました。  

https://pepabo.connpass.com/event/280682/

飛び入りですが、「クリーアーキテクチャでのアンチパターン」というネタで話してきました。

LT の様子と資料はこちら

![LT風景](/images/articles/phpcon-fukuoka-2023-talk-impression/pepabo-lt.jpg)
*轆轤を回している感ｗ*

@[docswell](https://www.docswell.com/s/katzumi/ZJL8GX-clean-architecture-anti-pattern)

その後の懇親会では、初めてお会いする方が多かったのでご挨拶をしながら歓談していました。  
LT の内容で声がけして頂いけたのが、すごく嬉しかったです。 ^[博多に向かう高速バスで資料をまとめたかいがありました]  
内容的にアーキテクチャや人に依存する部分が多く、普遍性はないネタで伝わりづらいかなーと個人的に思っていました。  
ただ話をした方からは刺さったというご意見が多く、「あー自分だけではなかったんだ！」という安心感を得られました。

https://twitter.com/okashoi/status/1671847771534675968

普段感じていましたが、言語化できていなかったことをサラッと言って頂いて首がもげるほど同意しました。  
思ったよりも反響が良かったので、もう少し深掘りして言語化すると面白そうと感じました。^[「ActiveRecordの呪縛から開放（アンラーニング）するクリーンアーキテクチャのススメ」というタイトルだったら皆さん聞きたいでしょうか？ｗ]  
あと [きしだなおき](https://twitter.com/kis)さんとお話する機会を得られたのも個人的には嬉しかったです。  
きしださんは今年から ChatGPT ネタを積極的にブログにアップされていて、私自身が [OpenAIを使ったSlack botを作る](https://zenn.dev/katzumi/articles/introduction-of-slack-bot-with-app-manifest)際に参考にさせて頂いていました。著名な方とリアルにお話できるのはカンファレンスの魅力だと感じました。  
LT の内容を聞いてくださっていて、「モデルクラスが本当に必要なのか？」「モデルクラスがないとそれはオブジェクト指向とは言えないのか？」という質問を受けて ^[お酒が入っていて周りもガヤガヤしていたので、正確ではないかもですが] タジタジになってしまいました w  


## 前日

### 前日Meetup

PRTIMES さんにお邪魔してきました。

https://prtimes.connpass.com/event/280691/


昼の部のランチ会で runn ^[runnって何？という方は[こちら](https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn)の記事をご参照] の導入を検討されている方から嬉しいことにお声がけ頂きました！  
Form 認証がある Web アプリケーションで導入を検討されているということで、検証してみよう！ということでシナリオの作成に着手するなどしました。

https://twitter.com/BkNkbot/status/1672148975527546881


時間が空いてしまったので [@tyamahori](https://zenn.dev/tyamahori) さんと EngineerCafe にお邪魔してきました。  

![アンダースペース](/images/articles/phpcon-fukuoka-2023-talk-impression/engineercafe.jpg)
*こんなハイカラな空間が無料で使わせて頂けるなんて素敵*
https://engineercafe.jp/ja/


大変雰囲気のある歴史的建造物でもくもくする環境として最高でした。

自分も LT ネタを準備しつつまったり。。
検証用に [HTTPBin](https://httpbin.org/) をコンテナ実行しようとしていたら tyamahori さんに言われるがまま [OrbStack](https://orbstack.dev/) を install するなどしました。



PRTIMES さんの夜の部に戻り、 [@tadsan](https://twitter.com/tadsan)さんと [@uzulla](https://twitter.com/uzulla)さんのセッションを聞いてきました。

https://tadsan.fanbox.cc/posts/6216133

[@tyamahori](https://zenn.dev/tyamahori) さんの LT をみつつ、他の人の意見を眺めるなどしました。  

@[docswell](https://www.docswell.com/s/tyamahori/ZGXQ1L-2023-06-25-112956)

自分に近い所の LT ネタに対しての反応を見ているとすごい新鮮でした。  
自分も素振りで LT 飛び入りしようかと考えていましたが、タイムオーバーでした。

### 前夜祭

次はハシゴで LINE さんにお邪魔しました。  

https://connpass.com/event/282285/

PRTIMES さんからの移動で [@catatsuy](https://zenn.dev/catatsuy) さんにタクシーをご一緒させて頂きましてありがとうございました！　
第 2 部からでしたが、充実の 3Track で内容もバラエティに富んでいてよかったです。  

解散して皆が繁華街に消えていくなか、自分はホテルに戻ってスライドアプリの使い方の復習をしつつ微調整をおこなっていました。

https://twitter.com/katzchum/status/1672240070739492864

今回からスライドは [sli.dev](https://sli.dev/) で Markdown で記載するようにしていたのですが、Export した PDF がなぜか speakerdeck にアップロードすると `We had issues processing your talk` と表示されて悩まされました。。 ^[原因わかる方いらっしゃいましたらアドバイス頂けますと幸いです。zenn.devでDocswellのスライドの埋め込みが対応してなくてつらいです :sob:]

タイムラインが飯テロ過ぎて悶えましたが、必死に耐えていました w

## 当日

### VAddyホール

最初のセッションはこちら。  

https://speakerdeck.com/kotomin_m/chuan-etai-ohurainnokanhuarensunican-jia-surumeritutotocan-jia-sitekara200-percent-le-simutamenishi-jian-sitehosiikoto

前日にお話させて頂いた際にすごい練習されていて喉を涸らされていました。  
同じ初登壇ということで勇気をもらおうと聞きいていました。  
すごいカンファレンス愛を感じる内容で、パワーを頂きました。  
声量とか勢いとか大事！ ^[すごい聞きやすく声が通っていて、なるほどこれは喉にくるなと感じましたｗ]


次はスタヰルさんのセッション  

https://speakerdeck.com/stwile/huremuwakugasheng-michu-sufu-zhai-yafu-za-sanidui-site-phpunittofu-kihe-tuteiku

テストしたい一単位をチーム内で話し合うというすごい深い内容でした。  
スライドにテストピラミッドが出てきて自分が今日話す内容と一部同じかも。。と思い Ask the Speaker でお話させて頂きました。 
色々お話を伺えて今のプロジェクトと似たような環境で課題感も同じっぽさを感じました。  
違うアプローチで取り組まれている内容を深くお聞きでき、すごい参考になりました。 

お昼が取れなくなりそうだったのでサク飯

https://twitter.com/katzchum/status/1672447432947560448

### フリースタイル登壇

景気づけにフリースタイル登壇に LT をねじ込みましたｗ  

![フリースタイル登壇](/images/articles/phpcon-fukuoka-2023-talk-impression/freestyle-speak.jpg)
*各種ロゴステッカーを貼って気合を入れました*
@[docswell](https://www.docswell.com/s/katzumi/57VJVG-github-actions-custom-actions-tips-fy2023)

内容は推し OSS を中心に、小ネタをお話させて頂きました。

次は Symfony のお話

https://speakerdeck.com/ippey/symfony-6-dot-3noosusumexin-ji-neng-matome

普段は Laravel をクリーンアーキテクチャでうすーく使っている（これもオレオレ FW かも）のですが、Symfony のコンポーネント恩恵設けているので気になります。  
Attributes 祭りになっているのは、今後どんどんエコシステムが広がりそうな感じをうけて俄然 Symfony に興味が出てきました。　　
いくつか質問させて頂きましたが、凄く期待できると思いました。  
Attributes から OpenAPI の Spec が出力できるとかなり良さそうと思いました。 ^[ご存知な方がいましたら教えて頂けますと幸いです]


### 登壇〜Ask the Speaker

いよいよ登壇です！  

![登壇風景](/images/articles/phpcon-fukuoka-2023-talk-impression/hall_vaddy.jpg)
*めちゃめちゃ緊張していました*
@[docswell](https://www.docswell.com/s/katzumi/5EN8N1-10-reasons-to-write-api-scenario-tests)

待ち時間がすごく長く感じましたが、始まるとあっという間でした。  
登壇席からセッションを見に来てくれた方の反応を見れれば良かったのですが、照明が眩しかったのとそれほど余裕もなかったので顔をあまりあげれませんでした。^[スライド用のリモコンが欲しいと感じました。sli.devでも使えるリモコンってあるんでしょうか？]  
昨晩資料見直したのと、他の登壇者のセッションを見て伝えたい気持ちが膨らんでしまったのか、まとめの最後のスライドが尻切れになってしまったのが残念でした。  

Ask the Speaker では、数名の方にご質問を頂きました。  
本当にありがたかったです。  
runn の開発者である [@k1low](https://twitter.com/k1low) さんもお声がけ頂けて感激でした！  

https://twitter.com/k1LoW/status/1672475802087333890

runn のマニアックな使い方の記事を書こうかなーと思った一幕でした。

### VAddyホール (15:15 〜 )

ほっと一息ついたあとに参加したセッションはこちら

https://speakerdeck.com/k1low/phpconfuk-2023

tbls すごく便利に使わせて頂いています！  
5 年も継続的に開発され続けていて凄いです！
そしてセッションの発表内容の安定感が凄くて流石と感じました。　　
システムとドキュメントの乖離をゼロにするというのは翌日のスキーマ駆動開発の LT にもつながるものがあり、更にその上を目指す取り組みがすごく大変刺激を受けました。  
ドキュメントとしてだけ扱うのは勿体ないという強い思いから、より付加価値を与えることでよりしっかりとしたドキュメントを作成しようとインセンティブを与えるサイクルができて良いなと思いました。  
LLM を組み込んだ取り組みもされていて、もしかしたら runn に組み込まれて自動的にリクエストパラメータの組み合わせが生成されていく将来が出てくるのでは？と妄想するなどしました。

### Dホール

https://koyhogetech.hatenablog.com/entry/20230625/phpconfuk

スライドがなかったので、こちらをリンクさせて頂きます。  
ホール D 席がすべて埋まるぐらい注目度が高かったと思います。　
[ひさてるさん](https://twitter.com/tanakahisateru)との対談が面白かったです。  
きしださんとの対戦？はまたの機会を期待したいと思います。  

### 懇親会 〜 N次会(runn開発者会議Day1)

LT とかクロージングとか紹介すると長くなりすぎるので割愛させていただきます。

お楽しみの懇親会に突入です。

https://twitter.com/katzchum/status/1672566047298441218

乾杯のあとに @k1Low さんと runn の改善についてお話をさせて頂きました！  
やはり対面でお話させて頂きながら意見交換できるのがいいですねー。  

PHPerKaigi 2023 での[やり取り]((https://zenn.dev/katzumi/articles/runn-developers-conference-in-phperkaigi2023#%E3%81%BE%E3%81%A8%E3%82%81))のフラグを回収出来てよかったです。

![フラグ](/images/articles/runn-developers-conference-in-phperkaigi2023/tweet2.png)
*PHPerKaigi2023が終わってからのやりとり*

懇親会でも色々な方とお話させて頂いて楽しかったです。  
話をした内容を忘れないように 2 次会の移動中にツイートしている自分偉い！

そのあと３次会まで盛り上がった福岡の夜でした。

## 翌日(runn開発者会議Day2)

### After Hack!!

https://fusic.connpass.com/event/274365/

Fusic さんのもくもく会に参加してきました。  
皆さんが、カンファレンスの記事をモクモクと書いている中、自分と k1Low さんは runn の拡張についてディスカッションしていました。

https://twitter.com/katzchum/status/1672782317918760960

帰りの時間が近づいたので最後にプロポーザルネタを LT させて頂きました。

@[docswell](https://www.docswell.com/s/katzumi/ZJLX7X-schema-driven-development-flow)

こちらのミッションもカンファレンス駆動で完遂することが出来てよかったです。  

https://twitter.com/k1LoW/status/1644986256324202498

uzulla さんから「やっている開発プロセスでトラブった場合はどうするのか？（意訳」とご質問頂きました。 
その場では「Validation のロジックはごくシンプルな１つの Trait になっているので問題になりづらい」と少し外れた回答をしてしまっていたような気がします。  
補足としてこの場で回答を付け加えるとしたら以下のようになります。  

* FW のルールを OpenAPI の Spec 化するのでなく、より普遍的な OpenAPI のルールを FW の Validation ルールにマッピングするので問題が起こりづらい  
FW と比べると OpenAPI のルールが少なくバージョンアップも少なく、FW のバージョンアップの影響を受けづらい  
* FW のアプリケーションレイヤーのコード生成（Validation 文字列を組み立てる）するフローで直接的に FW に依存や拡張しておらず、いつでも辞められる（＝独自のアプリケーション部分を捨てやすい）  
独自ルールやコマンドを使わなくなったとしても以前のように手動でルーティングや Validation を書くことは継続してできます。  
実際に独自 Validation ルールは上書きもできるようになっているので、独自ルールを外れる場合でも対応可能です。 

https://twitter.com/katzchum/status/1672848369151270913

今回の runn 開発者会議のアウトプットです。  
ホワイトボードを使っての議論はやっぱり開発者体験がすこぶる良かったですｗ

### 帰路

家につくまででがカンファレンスです。

![博多やりうどん](/images/articles/phpcon-fukuoka-2023-talk-impression/imafukuokaniiruphper.png)
*博多やりうどん*

[@muno92](https://twitter.com/muno_92) さんとばったりお会いして飛行機も結局同じだったりｗ
After Hack のあとの LT の内容も教えて頂くなどしました。

## 最後に

本当に今回のカンファレンスでは色々なご縁で登壇させて頂くことになったのですが、参加することで新しいご縁が広がった感を凄く感じました。  
そのご縁が凄い温かい雰囲気の中によるもので、凄くほっこりしました。  
運営並びに登壇者の方々、そして参加された方にこの場をお借りして感謝をお伝えしたいと思います！  

カンファレンス駆動で動いた結果、色々な繋がりが出来て新しい価値が生まれてくのは大変恵まれた良いサイクルだと感じています。  
今回提出していた 5 つのプロポーザルの内、4 つをお披露目できたことは自分自身も驚きました。  

個人的にもう少し遡って去年から振り返ってみると「推しツールに出会い全面的に推していっていたら、登壇することになっていた！」という感じがあります。  

https://twitter.com/tanakahisateru/status/1673314814096211969

新しい推しの潮流が生まれつつあるなーと感じています。  

@[docswell](https://www.docswell.com/s/tyamahori/598RVW-2023-06-25-113100)

多分震源地はここｗ