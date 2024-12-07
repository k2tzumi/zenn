---
title: "推しOSSに貢献してTシャツをもらおう【タイトル誤り訂正あり〼】"
emoji: "👕"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["hacktoberfest","oss"]
published: true
---

:::message alert
Hacktoberfest の報酬の見直しがおこわなれるようです。  
sustainability があるイベントとする為に報酬が見直しされ、これまでの T シャツのリワードから、デジタルリワードキットに移行する予定とのことです。 :bow: * 2
:::

:::message alert
Hacktoberfest のルールが厳密化されており、任意の公開リポジトリにプルリクエストを送るだけではなく、参加するリポジトリが Hacktoberfest に登録されているか、またはプルリクエストに `hacktoberfest-accepted` というラベルが付けられているかを確認する必要があることが確認できました。  
すでにプルリクエストを送ってしまわれた方大変申し訳ございません。記事を訂正してお詫びいたします :bow:
:::

## はじめに

皆さん推し OSS はありますか？  
最近自身の推し OSS が 200Stars を超えたりしました。

https://twitter.com/k1LoW/status/1705769690033979501

またその OSS がハッカソンで使われていて感動したりしました！

https://twitter.com/Selria1/status/1705594010335842604

本記事は普段お世話になっている OSS へのお礼も含めて貢献してみませんか？という内容です。

来月（10 月）ならなんと ~~Tシャツ~~ デジタルバッジが貰えます！

:::message
~~Tシャツ~~ デジタルバッジを貰うには手続きと条件を満たす必要があります
:::

## Hacktoberfest 2023

https://twitter.com/katzchum/status/1707954447505379544

オフィシャルのページはこちら

https://hacktoberfest.com/

こちらのイベントですが、AI さんに [説明させると](https://sl.bing.net/knPyk4D9YWW)

```
このイベントはオクトーバーフェストにかけています。
Hacktoberfestという名前は、October（10月）とHack（プログラミングの俗語）を合わせた造語です。
オクトーバーフェストはドイツのミュンヘンで開催される世界最大のビール祭りで、10月に行われます。
Hacktoberfestは、オープンソースのプロジェクトに貢献することで、
ビールではなくTシャツやバッジなどのデジタルな報酬を得られるイベントです。
Hacktoberfestの趣旨は、オープンソースのコミュニティを支援し、スキルを磨き、新しい人とつながることです。
Hacktoberfestは2023年で10周年を迎え、これまでに数千人の開発者が参加しています。
```

らしいです。  
説明にもありますが、10 年の歴史があるので安心してください。　　
自分も去年から参加していますが、T シャツを Get できました

:::message alert
Hacktoberfest2023 からはデジタルバッジになります！  
物理的な報酬では国によっては関税や輸入関税を負担する必要があったみたいです。
:::

https://twitter.com/katzchum/status/1622538122671321088

開催期間は 10 月 1 日から 10 月 31 日までとなりますが、結果発表から発送は大分遅くなりますｗ

### デジタルバッジについて

https://www.holopin.io/

Holopin は、デジタルバッジを作成、発行、収集できるプラットフォームです。
Holopin のバッジは、スキルや実績など、さまざまなことを証明するために使用できます。

https://www.holopin.io/@k2tzumi#badges

こんな感じのバッジを貰えます

![badges](/images/articles/hacktoberfest-2023/holopin-badges.png)


## Hacktoberfestの参加ルール・概要について

1. 申し込み（アカウント登録）する
2. 10 月 1 日から 10 月 31 日までの間に、GitHub または GitLab 上の公開リポジトリに 4 つ以上のプルリクエストを送る
3. 参加者は、先着で自分の名前で木を植えてもらうか、Hacktoberfest2023 の　~~Tシャツ~~ デジタルリワードキットをもらうことができる

ざっくりした概要としては以上です。  
申し込み手順やプルリクエストの条件等を以下にまとめます。

## 申し込み手順

以下に申し込み手順をまとめます。

1. [Hacktoberfestの公式サイト](https://hacktoberfest.com/) にアクセスします。
2. 右上の「[START HACKING](https://hacktoberfest.com/auth/)」ボタンをクリックします。
3. GitHub または GitLab のアカウントで認証を開始「INITIATE」します。
GitHub または GitLab のアカウントでログインします。アカウントを持っていない場合は、事前に作成しておいてください。
いつもの OAuth をしてください。

![OAtuh画面](/images/articles/hacktoberfest-2023/sign-in-to-github.png)

4. 認証後、プロフィールページが表示されます。ここで、参加規約に同意し、メールアドレスや参加方法などの必要な情報を入力します。  
:::message
必須項目はメールアドレスの確認と参加規約のみの同意だけです
あとは Optional なのと、登録後の [プロフィールURL](https://hacktoberfest.com/profile) の「[EDIT INFO](https://hacktoberfest.com/profile/edit/)」から変更可能です
以下は内容の説明です。
:::
  - プロフィール変更  
プロフィール画面が出れば OK です  
![プロフィールページ](/images/articles/hacktoberfest-2023/profile.png)
必要があれば名前を変更してください
  - How will you be participating?（どのように参加されますか？）
![参加方法](/images/articles/hacktoberfest-2023/participating.png)
コントリビューター(CONTRIBUTOR)かメインテナー(MAINTAINER)のどちらかを選びます
コントリビューターはプル/マージリクエストを送る側での参加になります。
メインテナーはリポジトリを管理する側での参加になります。
自身が OSS 開発してている場合はメインテナーとして行うのもありです。その場合にリポジトリに [#hacktoberfest](https://github.com/topics/hacktoberfest) の topics を付けます。そうすると参加者からプルリクエストを貰えるかも知れません。
  - What is your experience level?（あなたの経験レベルは？） 
![経験レベル](/images/articles/hacktoberfest-2023/experience.png)
初心者(NEWBIE)、熟知者(FAMILIAR)、経験豊富(EXPERIENCED)のいずれかを選びます。 
おそらくこの記事に興味を持たれた方は、大半は NEWBIE かと思います。 
ハクトーバーフェストとオープンソースへの貢献が初めての場合は NEWBIE を、
経験がなくても自身がある方は FAMILIAR を、OSS の開発者若しくは貢献をされた方は EXPERIENCED を選ぶといいかと思います。
  - How would you like to contribute?（どのように貢献したいですか？）  
![貢献方法](/images/articles/hacktoberfest-2023/contribute.png)
コード(CODE)又はコード以外(NON-CODE)を選びます。
バグフィックスや新機能など、オープンソースへのコード貢献する場合は CODE を、
コード以外での貢献（テクニカルドキュメントの作成や翻訳やプロジェクトへの寄付）は NON-CODE を選びます
  - 諸々アンケート
![アンケート](/images/articles/hacktoberfest-2023/questionnaire.png)
    - Are you currently enrolled as a student?（現在、学生として在籍していますか？）
    - Are you interested in contributing to AI or Machine Learning projects?（AI や機械学習プロジェクトへの貢献に興味がありますか？）
    - Which of the following best describes your role at your primary employment?（あなたの主な職場での役割について、最も適切なものはどれですか？）  
    - What country are you participating from?（どの国からの参加ですか？）
  - MARKETING OPT-INS
  マーケティングオプトインの受け入れを希望しますか？しない場合は DON’T EMAIL ME にチェックをしてください
  - RULES & TERMS（規約同意）
![規約同意](/images/articles/hacktoberfest-2023/rules.png)
趣旨に賛同し、ルールを理解したらすべてチェックしてください。
最後に「REGISTER」を押して完了です
![登録](/images/articles/hacktoberfest-2023/register.png)
5. 完了画面が表示されたら OK
![進捗画面](/images/articles/hacktoberfest-2023/progress.png)
プロフィール画面で以下のような画面が表示されれば OK です。
PROGRESS が `0 / 4` になっていますが、プルリクエストがあと 4 つ送ればゴールになることを示しています。
こちらの画面は [プロフィール画面](https://hacktoberfest.com/profile/) からいつでも確認できます。

## プルリクエスト（マージリクエスト）を4つ送る

上記申込が完了すれば 10 月 1 日から 10 月 31 日までに有効なプルリクエスト（マージリクエスト）を 4 つおくりましょう。  
期間中に達成していれば、プロフィール画面のリンクからバッジなどのデジタルな報酬を受け取るの他に

* Hacktoberfest の ~~Tシャツやステッカー等~~ デジタルリワードキットを貰う
* 自分の名前で木を植えてもらう

が選べるので配送先の住所を登録します。
報酬は先着順となり在庫がなくなり次第終了となるそうなので、早めに申し込むといいらしいです。

### よくある質問

Q: プルリクエストは何件でも送っても良いの？
A: はい。4 件以上も送っても問題ありません。
ただスパム的な内容にならないように価値のあるプルリクエストを送りましょう。

Q: どんなプルリクエストでもいいの？
A: スパムは駄目ですが、コード以外でも Documents の修正でも問題ないです。 
プロジェクトのメンテナーに承認されれば 1 件とカウントされます。

Q: マージされなくてもいいの？
A: マージされれば有効としてカウントされますが、マージされなくても 14 日間経過すると完了とみなされます。  
但し、拒否されたり無効とされたりした場合はカウントされません。

Q: どのリポジトリにプルリクエストを送ればいいの？
A: ~~パブリックなリポジトリならどれでもOKなようです。~~  
*Hacktoberfestに参加表明しているリポジトリに限られます。*
*表明されているリポジトリは [こちら](https://github.com/topics/hacktoberfest) で公開されていますので確認の上、プルリクエストを送ってください。*
ただし、各リポジトリでコントリビュートする際のお作法があればそれに従ってください ^[プルリクエストのテンプレートやコーディングスタイルやテストの実施など]

Q: 自分がオーナーのリポジトリにも送ってもいいの？
A: ~~パブリックなリポジトリなら問題ないです。~~
*自身のリポジトリでも Hacktoberfestへの参加表明が必要とななります。*
*topicsに `hacktoberfest` を追加してください。*
*若しくはプルリクエストに `hacktoberfest-accepted` というラベルを付けてください。*
ここだけの話ですが、zenn は Github 連携ができるようになっています。  
パブリックなリポジトリだと記事をプルリクエストにしてアップするだけでカウントできると思います w

Q: Hacktoberfest に参加表明しているリポジトリに限定したほうがいいのでは？
A: ~~限定はされていないです。ただし、表明されているリポジトリの方が趣旨を理解されているのでトラブルは少ないかもです ^[10月になると質の低いプルリクエストが増えることに不満を抱いている方がいらっしゃるみたいです。]~~
*限定されておりました。申し訳ございません。*
表明されているリポジトリは [こちら](https://github.com/topics/hacktoberfest) で公開されていますので参考まで

Q: 始めての PR で少し怖いのですが
A: はい最初は不安ですよね。そういう人の為にリポジトリが公開されています。
~~[firstcontributions/first-contributions](https://github.com/firstcontributions/first-contributions/blob/main/translations/README.ja.md)~~
[ossamamehmood/Hacktoberfest2023](https://github.com/ossamamehmood/Hacktoberfest2023)
こちらの記事が参考になるので、是非この機会にチャレンジしてもらえると良いかと思います
*リポジトリが限定されますので上記のリポジトリで読みかえてください*
https://qiita.com/e99h2121/items/a5c7b9a71555d064c687

Q: プルリクエストが受け付けられたか？不安
A: 登録したメールアドレス宛に info@hacktoberfest.com から `Pull/merge request approved` というメールがきます。

Q: プルリクエストがカウントされたかはどう確認しますか？
A: [プロフィール画面](https://hacktoberfest.com/profile/) から確認してください。
また承認されると登録したメールアドレス宛に info@hacktoberfest.com から `Pull/merge request #1 accepted!` というメールがきます。

## プルリクエストを送るのは難しいという方へ

自分は Hacktoberfest での経験はないのですが、寄付を送るというのもあるようです
https://hacktoberfest.com/donate/

Hacktoberfest 以外になりますが、GitHub Stars というのがあります。

https://stars.github.com/

GitHub Stars とは、GitHub の最も影響力のある開発者に感謝し、彼らの作品を紹介し、より多くの人に届けるためのプラットフォームを提供するプログラムになります。

以下のページから推薦できます。

https://stars.github.com/nominate/

日本では [matz](https://github.com/matz) さん含め 4 名がなられています。

https://stars.github.com/profiles/?country=Japan

あなたのお世話になっている OSS の開発者を推薦してみると良いでしょう。

## 最後に

自分は今まで Slack bot のリポジトリを公開するなどしてきましたが、外部の OSS リポジトリに対して貢献するということはしてきませんでした。
それが去年から推し OSS を見つけたことをきっかけに本当の意味での OSS 活動をするようになりました。  
その際に Hacktoberfest を知り、複数のリポジトリにプルリクエストを出すなどのモチベーションにもなりました。　
継続的に OSS に貢献する為にも Hacktoberfest はよい機会だと思っています。 
もし皆さんも少しでも気に入っている OSS があったら少し勇気を出してプルリクエストを送ってみるのが良いのではないでしょうか？
私は推し OSS にプルリクエストを送るなどしていたら、なんやかんやあってカンファレンスで登壇することになったのでオススメですｗ

登壇した際の記事はこちらになります。
https://zenn.dev/katzumi/articles/phpcon-fukuoka-2023-talk-impression