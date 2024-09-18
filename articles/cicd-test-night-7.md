---
title: "CI/CD Test Night #7で発表してきました"
emoji: "✅"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["cicicd","プラクティス","tagpr"]
published: true
---

## お詫び

3 ヶ月経過してしまっていますが、[CI/CD Test Night #7](https://testnight.connpass.com/event/311263/) の参加＆発表記事となります。
当時から本業が忙しく、オフライン参加ではなく録画配信でも大丈夫でしょうか？など調整のご相談をして頂いた [@ponkio_o](https://twitter.com/ponkio_o) さんありがとうございました！
では振り替えてっていきましょう。

## 業務で役立つか もしれない？！GitHub Actions Tips 集

[@ponkio_o](https://twitter.com/ponkio_o) さんの発表。

@[docswell](https://speakerdeck.com/ponkio_o/ci-cd-test-night-number-7)

matrix job で JSON 使えるというのは初めて知りました。また照会にあった secrets を使ったやり方も使えそうだなと思いました。  
paths フィルターは便利に使っていましたが、使い方は悩むところが多く、フィルタリングの Job を定義するやり方を照会されていたので、やっぱりやりたくなりますよねーという印象を持ちました。

## CI/CD のセキュリティや Developer Experience を改善するツールやプラクティス

[@szkdash](https://twitter.com/szkdash) さんの発表。

https://docs.google.com/presentation/d/1rN4kTtvErrheZ3SXNr49XUHbiGIoorpfiLGXjLt5vsc/edit#slide=id.g2c674150394_0_0


GitHub Actions のセキュリティベストプラクティスは大変為になりました。  
ツールのバージョンを固定するのはやったほうがいいけれど、なかなかハッシュ値までやるのは大変だなーと思っていましたが、
各種ツールで変換・確認し、Renovate で自動更新まで行うというフローが大変学びになりました。

## CI/CD があたりまえの今の時代に API テスティングツールに求められていること

[@k1LoW](https://twitter.com/k1LoW) さんの発表。

https://speakerdeck.com/k1low/cd-test-night-number-7

「テストケースに ID がある」という要素は昨年６月の [PHP Conference Fukuoka After Hack!!](https://fusic.connpass.com/event/274365/) にて議論して実装された内容でした。  
その時の議論した際にホワイトボードへ書きなぐった内容がこちらになります。

https://x.com/katzchum/status/1672848369151270913

ID があるとトレーサビリティをもたせることができるので大変便利です。

こちらの id の機能を使ってカバレッジを計測する方法を「API テストでもカバレッジ測定したい！」という LT で発表したので、興味のある方はどうぞ。

@[docswell](https://www.docswell.com/s/katzumi/ZW17J8-how-to-measure-api-test-coverage#p29)


## リリース戦略を支える CI/CD パイプライン

最後に発表した内容のスライドはこちら。

@[docswell](https://www.docswell.com/s/katzumi/58G8J9-empowering-release-strategies-cicd-pipelines)

内容は tagpr を使ってリリースマネジメントを民主化したよ！というのをデモも交えて発表しました。

tagpr は runn の開発時に知ったのがきっかけてプロジェクトに導入して本当によかったと感じています。  
tagpr が一番良いなと感じている所が、リリースが見える状態になって、そこから CI/CD の workflow をフックするポイントとして作り込みができる点です。
今回の発表で tagpr と連携して組み込んだテスト等のアクションを紹介できてよかったです。

モブで CI がうまく動かなかったのは焦りましたが、ご指摘を頂いていた通り連続的に PR をマージした際にベースブランチが変わる前にマージしてしまったからでした😅。

https://x.com/szkdash/status/1772585659984654337


## 懇親会

当日は結構な雨が降っていたのですが、その中でも参加されていた熱心な方々ということもあってか熱い議論がされていた印象でした。  

自分も何名かとお話をさせて頂いて、バラエティ豊かなプロジェクトと CI も GitHub だけでなく、GitLab も使っている話を聞いて楽しい時間を過ごせました。

# 最後に

今回、発表できる機会を頂いて光栄でした。  
しかも [@k1LoW](https://twitter.com/k1LoW) さんと同じ会でお話できたのも感慨深いものがありました。  
お声がけ頂いた [@ponkio_o](https://twitter.com/ponkio_o) さん、重ねてありがとうございました！
