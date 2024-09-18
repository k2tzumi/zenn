---
title: "PHPカンファレンス沖縄2023で登壇してきました"
emoji: "🌺"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phpcon","conference","iwillblog","クリーンアーキテクチャ"]
published: true
---

## PHPカンファレンス沖縄2023で登壇してきました

6 月の福岡に続いて ^[福岡の内容は[こちら](https://zenn.dev/katzumi/articles/phpcon-fukuoka-2023-talk-impression)です。] PHP カンファレンス沖縄 2023 でも登壇してきました。

https://phpcon.okinawa.jp/

https://twitter.com/phpcon_okinawa/status/1703263072146817075

## セッション内容

スライドはこちら
@[docswell](https://www.docswell.com/s/katzumi/Z989R9-activerecord-pattern-unlearning-clean-architecture)

[このトークの時間帯のXポスト](https://twitter.com/search?q=since%3A2023-09-16_15%3A30%3A00_JST%20until%3A2023-09-16_16%3A15%3A00_JST%20%23phpcon_okinawa%20%23track_b&src=typed_query)
[Discordチャネル](https://discord.com/channels/1150751595089371290/1150782186568556594)

ActiveRecord パターンについて PoEAA とかの内容を整理したり、SOLID 原則について改めてクリーンアーキテクチャでの位置づけを言語化したり、個人的には調べ物が多くて苦労をしました。 
ただ整理して言語化する中で、漠然としていたものが整理できてやっぱり ActiveRecord って思想からしても強いなぁと実感しました。 
30 分という枠でしたが、クリーンアーキテクチャの触りを触れただけでもボリュームが結構大きく最後の方は駆け足になってしまいました。  
福岡での反省もあり、素振りを結構行っていたのですが、それでも当日は欲が出て言及する内容が増えてペース配分にブレが発生してしました。


## 裏話

### トーク内容のネタについて

今回のセッション内容ですが、実は福岡での前夜祭の LT が元ネタになっています。  

@[docswell](https://www.docswell.com/s/katzumi/ZJL8GX-clean-architecture-anti-pattern)

インスピレーションを頂いたのが、こちらの Post でした。

https://twitter.com/okashoi/status/1671847771534675968

[前回の記事で前振りしていた](https://zenn.dev/katzumi/articles/phpcon-fukuoka-2023-talk-impression#fn-e640-2)のが、実現した格好でした。

### 話さなかったこと

枠の関係で削ってしまったネタがあるので、こちらに書かさせて頂きます。
クリーンアーキテクチャでのアンチパターンの1つとして

* エンティティ識別子に auto increment な id を使う

というのがありました。  
これも ActiveRecord の標準では id にしてしまっているのに引きづられているなぁと感じていました。
せっかくクリーンアーキテクチャにしたので ID 生成器　^[Snowflake形式のID. Wiki: https://en.wikipedia.org/wiki/Snowflake_ID https://github.com/godruoyi/php-snowflake PHPの実装だと https://github.com/godruoyi/php-snowflake があります]を使って Code を採番するのが良いと考えています。 
将来的な分散システムにした際に id だと大変面倒なことになります。

### チームLITALICO

今回は同僚の [ちゃまほり（@tyamahori）](https://zenn.dev/tyamahori) さんと一緒に登壇していました！

https://twitter.com/tyamahori/status/1703585336851563007

今回の登壇のきっかけとしては、これですｗ

https://twitter.com/tyamahori/status/1675886047950172162

沖縄で対戦することになりました

## 懇親会

登壇者や参加者の方々と貴重な話ができて今回も大変楽しかったです。 

神ツールの作者である [しめじ（@smeghead）](https://twitter.com/smeghead) さんとリアルで話ができて嬉しかったです。
https://twitter.com/77web/status/1702880224961273941

ちょっとまだ使いこなす所まではできていないですが、[php-class-diagram](https://github.com/smeghead/php-class-diagram)に大変期待しています。

また同じ TrackB で PoEAA で紹介されている Unit of Work パターンについて発表された [しまぶ（@shimabox）](https://twitter.com/shimabox) さんともお話させて頂きました。同じく ActiveRecord について取り上げられて、パターンの説明はやっぱり難しいですよねーというお話をしました。

[さぼ（@saboyutaka）](https://twitter.com/saboyutaka) さんに声がけを頂きセッションの感想を頂けたのもありがたかったです。  

[やまゆ（@yamayuski）](https://zenn.dev/yamayuski) さんと負荷テストツールの意見交換を行いました。ちゃっかり runn の宣伝も行っておきましたｗ

## 後日談

しまぶさんの発表であった Doctrine についてすごく気になっていてポストしていたのですが、[ななうぇぶ（@77web）](https://twitter.com/77web)さんに補足頂きお声がけ頂きました。

https://twitter.com/77web/status/1703627848865763364

色々考えるきっかけになって大変ありがたかったです。

https://twitter.com/katzchum/status/1703656441373143435

さぼさんが、後日のイベント（Tech　Base Okinawa 2023）で発表された QraphQL のセッションで ActiveRecord と REST の関係についてスライドで触れられていました。
https://twitter.com/saboyutaka/status/1705637361936429246

ヒント（気づき）になったという言葉を頂いて、本当に嬉しかったです。

## 沖縄

今回始めて沖縄に来たのですが、景色も良く、食べ物も美味しかったです。  
前職の同僚にも会えたのも良かったです。  

https://twitter.com/katzchum/status/1702590267889766597

https://twitter.com/katzchum/status/1703245444552552789

https://twitter.com/katzchum/status/1703246495284351195

https://twitter.com/katzchum/status/1703258392150839740

https://twitter.com/katzchum/status/1703316906395206011

色々堪能できたのですが、心残りが 2 つありました。

福岡のラーソーメンに続き沖縄では締めステーキを食べれなかったので、次回のお楽しみにしたいと思います。  
リハビリ中だったので、乗れませんでいたがロードバイクで是非走りたいと思いました！

## 最後に

今回も大変楽しく充実したカンファレンスでした。 
次に繋がる面白い話も聞けたし、また機会があれば是非沖縄にいきたいと思います。