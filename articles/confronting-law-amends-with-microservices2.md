---
title: "法改正をマイクロサービスで立ち向かう（後編）"
emoji: "🙆"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["microservice","ddd"]
publication_name: "litalico"
published: true
---
:::message
この記事は [LITALICO Engineers Advent Calendar 2021](https://qiita.com/advent-calendar/2021/litalico)のカレンダー2 の 12/24 配信になります。
:::

## はじめに

ボリュームが多くなってしまったので前編・後編の 2 部構成になり今回は後編となります。  
前回の記事は [こちら](https://zenn.dev/katzumi/articles/confronting-law-amends-with-microservices)  
Engineers Advent Calendar なのでエンジニア向けの記事になります。

## お題

前回の記事で法改正に対応するにあたりドメインをシンプルにする為に

```
同一の関心事を時系列で分断させ別の関心事として扱う
```

という、ドメイン分割するアプローチについて説明させて頂きました。  
稼働中のサービスに対してドメイン分割を行う際にどう取り組んで実現していったのか、具体的に検討したことや課題も含めて書きたいと思います。  
マイクロサービスのドメイン分割する際の事例として参考になれば嬉しいと考えています。

実現したいイメージは以下になります。  
前提として SPA として構築しているものとなります。  

* As-Is  
![As-Is](/images/articles/confronting-law-amends-with-microservices/as-is.png)

* To-Be  
![To-Be](/images/articles/confronting-law-amends-with-microservices/to-be.png)

## まず考えたこと

今回分割する関心事は元々1つです。具体的にはレセプト業務の関心事のドメインを扱う API（以下レセプト API）があり、その API を水平分割するイメージです。  
レセプト業務のフロントエンドアプリ以外からも複数の業務（フロントエンド＆バックエンド）が呼び出されます。  
時系列で処理する API を呼び分ける必要があるのですが、まずどうやって呼び分けるか？ということを考えました。

呼び出し元のアプリ側でも同じ時系列のキーは持っており、呼び出し元で判断して呼び出し先の API を変更することは可能です。
ただ以下のシステム特性があり、方針の検討が必要だと考えました。

* 分割する API に対して呼び出し元が多岐にわたる  
SPA で構築されており、バックエンドは PHP Laravel、フロントエンドは vue, nuxt(javascript)と言語・フレームワークも複数存在し依存しているアプリの種類も多い ^[必然と関連するチームも多くなります]
* 分割される API が今後も増え続ける  
3 年毎にドメイン分割していきます
* プロダクトとして機能追加は活発である  
法改正でも API 分割と同時に新規 API の追加があります

また以下も懸念としてありました。

* 改修範囲が広く、リソース的に修正できるか？  
法改正の改修中に他チームに依頼して対応可能だろうか？
* 分割後の API が最初は存在しないが、いつから改修を始めるか？  
分割後の API の完成をまってから、呼び出し元の改修〜検証まで行うのは難しいのでは？
* 言語・フレームワークも複数あり対応漏れや品質のばらつきが発生しないか？  
まず影響調査が必要ですが、対応漏れが一番怖いです。  
同じ API 呼び出しでフロントエンドとバックエンドとで挙動が変わったら嫌すぎます。。
* レセプト API の数が多く検証どうやってやろうか？  
レセプト API は 75 ほどあり、分割した後でもロジック的には変わらない API もあり、正しく呼び分けられているか？を1つづつ確認するのが大変です。    
法改正のメイン改修でリソースが割かれて、1つづつ検証する余裕はなさそうです。
* 法改正で追加される API やリリース以降で追加される API で対応漏れを発生させてしまうのでは？  
追加される API に対して確実に振り分けできるようにしたいです。
* 次の法改正でも同じ対応コストがかかる？  
また３年後の法改正でバタバタしたくない。今回の対応によっていい感じにして今後のメンテナンスコストを下げたいです。

前編で法改正への対応の難しさについて触れましたが、時間的制約があるので法改正のメイン開発に着手する前までになんとか見通しを立てたいと思いました。

## 誰がAPIを呼び分けるか？

API の振り分けの責務はどこに持たせるべきか？という点について考えたいと思います。  

* いつから法改正が施行されるか？
* それを知っているのは誰でその責務を持つのか？

上記を考えた際に

1. レセプト API が自身の扱う制度とその適用期間は把握している  
2. 逆にレセプト API 以外の業務では関心の範囲外  
3. 分割後の新・旧のそれぞれのレセプト API はお互いの扱う制度の適用期間は知らなくていい  

と考えました。

特に 2 については強く認識しているものです。

* レセプト業務の関心事が別領域に分散してしまう
* 呼び出し元側で振り分けるビジネスルールを組み込みたくない

それぞれ上述した懸念につながるもので、絶対避けないといけないものです。
そこで `それを知っているのは誰でその責務を持つのか？` については `レセプトAPIに近いレセプトAPI以外の何か` と考え

```
振り分けルール自体を新しい一つの関心事
```

として切り出すことにしました。  
こうすることで、レセプト業務に関連している他の業務にレセプト業務の関心事を分散させず一箇所に閉じ込めれると判断しました。


## 検討した振り分け手法

振分けルールを一箇所にとめて実現させる方法として以下の 3 つが候補としてありました。

1. 振り分けの為の時系列のキー情報を API のエンドポイント URL のパスパラメータとして定義し振り分けを行う  
https://example.com/api/rezept/${振分けキー}/calculate  
みたいな感じです。ルールベースにすることで、呼び出し元側では裏で動いているアプリケーションは意識しなくて済みます。  
パスパラメータであれば Amazon API Gateway や Nginx のレイヤーでも振分けを行えるメリットがありそうです。
2. 振り分けルールをモジュールとして実装し、各アプリに展開させる  
各アプリで共通で利用できる submodule が既にあり、そこのモジュール内にロジックを閉じ込めてしまう方法です。  
サービスプロバイダー経由で API アクセスさせたり、HTTP クライアントからの API 通信を hook させたりできます。  
直接 API の呼び出し先を変更するのでパフォーマンスや障害発生ポイントが少ないというメリットがありそうです。  
3. Proxy アプリケーションを作成し、そこで振り分けを行わせる  
実装のコストがかかりますが柔軟性があります。  
ただ、API の数が 75 ^[法改正対応自体でAPIも増えることも想定されました] あり、スケジュール的な制約の影響を受けやすいデメリットが考えられました。  

上記 3 つの候補の実現性と優位性を確認する為に、まず現状把握を行いました。

* API 仕様書を精査  
* 呼び出し元のリスト化  

現状把握を行った結果、以下のことがわかりました。

* 単純振分けでは実現できない  
大きく分類すると 5 パターンがあり、それぞれ対応を行う必要がありました。
* 呼び出し元となる影響をうけるアプリがフロント・バックエンド両方にそれなりにある  
ある程度想定はしていましたが、やはり数が多く、多岐に渡っていました。

上記の結果から、以下の方針としました。

```
Proxyアプリケーションを実装する
個別振分けのパターンをそれぞれ実装し、個別振分けのパターン以外については共通の振分けルールとして実装する
```

以下のようなイメージとなります。

![rezept-router](/images/articles/confronting-law-amends-with-microservices/rezept-router.png)


新しく追加した Proxy アプリケーションのアプリ名はレセプト API へのルーティングのみを行うので `rezept-router` と命名しました。 ^[責務も明確になっているので個人的に気に入っています]

こちらの構成にすることで、呼び出し元は今まで通り言語やフレームワークに関係なく API を呼び出しできます。  
また呼び出し元の対応も必要最小限で、今後追加される API に対しても対応漏れが発生しづらくなります。  
次の法改正でも `rezept-router` に振分け先アプリを追加するだけ済みます。

## rezept-routerの実装

rezept-router で実現する機能としては「リクエストパラメータに含まれる時系列のキー（提供月）を判断してリクエストを振り分ける」です。  
この提供月の指定のパターンと、レスポンスの扱いによって振分け処理が異なってきます。  
API は以下の５つのパターンに分類されると整理しました。

1. 提供月が1つしか存在せず、単純な振り分け  
2. 提供年月が複数にまたがり、更新 API を実行しステータスコードのみ返却する  
3. 提供年月が複数にまたがり、レスポンスを統合する必要がある  
4. 提供年月が存在せず全バージョンの API を呼び出す必要がある  
5. 単純に提供年月が存在しない  

困ったのが 5 のパターンです。どうやって振り分けしたら良いのか？判別できません。  
ただユースケースの想定としては提供月に関連付けされていて、パラメータとして定義していなかっただけでした。  
対応としては API の I/F 仕様上のみ提供月を追加し、呼び出し元で指定するパラメータに提供月を付与する様に対応を行いました。  
対応の結果、5 のパターンが 1 のパターンとして統合され 4 つのパターンのみ実装すれば良くなりました。  
今後の API 追加する際のルールとして必ずパラメータに提供年月を含めることとしてチーム内に共有しました。  
1 のパターンであれば rezept-router 自身に手を入れなくても対応可能となります。  

また、呼び出し元のリスト化した結果、既に使用されていない API が存在したがあったので屠り作業 ^[APIの削除] を行いました。

それぞれ対応によって、パターンの整理・統廃合され、75 ある API のルーティングの定義は最終的に 18 種類の実装で済みました。

## rezept-routerへの切り替え

振分けの関心事がうまく切り出しができ疎結合に保つことができた為、当初懸念していた

* 分割後のアプリがない状態で並行開発ができるか？  
* 振分けの動作検証を行えるか？  
* 調査漏れ等で対応漏れが発生しないか？  

という点がそれぞれ

* 他のアプリへの影響も少なく、並行して開発が進められるようになった  
ビッグバンリリースにならなくて済み、法改正対応前に事前リリースできた。
* ルーティングの処理は Laravel API として実装でき単体テスト可能になった  
API のモック処理は [php-vcr](https://github.com/php-vcr/php-vcr) を使い実際のレスポンスをキャプチャしたものを利用して検証を行いました。
* 切り替え時の動作検証が楽になった  
.env を切り替えるだけなので検証観点の粒度がアプリ単位で済むようになりました。

上記で解消されました。  
他の検討した手法に比べてテスタブルになりました。事前に十分品質を確認でき、切り戻しも簡単になりました。  
リリースは段階的に行うことにし、更にリスク低減の為に以下の対応を追加で行いました。

* 法制度開始前に新アプリを呼び出されてもエラーになるようにマスターデータの削除、バリデーションの追加  
* 逆に法改正開始後に間違ってルーティングされてもエラーになるように、旧アプリのマスターデータの有効期限を変更した  

こうすることで、最悪振分け処理に誤りがってもエラーとして検知でき、データ不整合を防げます。  
関心事を分離してしまったので、関心事以外のリクエストが来た場合にエラーにする必要があります。  
一番怖いのは振分けが間違っていてそのまま誤請求になることなので、しっかりと対応を行いました。  


## 後編まとめ

前回の記事に書きましたが、ドメイン分割の方針は決まっていたものの、いざ蓋を開けてみると色々課題がありました。  
関心事を分割する上で検討や配慮が足りなかった点があったと思います。  
具体的には上記の振分けパターンの種類が多かった件で、1 つのドメインで複数の関心事にまたがってしまっていた点です。  
フロントエンドのユーザビリティ向上の為、複数提供月を跨いだり、提供月関係なく全てのデータを取得するユースケースがありました。  
それ自体は問題ないのですが、API の仕様として 1 回のリクエストで処理する仕様になっており、ドメイン分割にあたり関心事が追加され複数関心事を扱う結果になってしまいました。  
フロントエンド側で API の呼び出し回数が増えてしまっても提供月単位で処理を行う API 設計が望ましかったのでは？と思いました。  
あと、本記事のスペースの問題で詳細には書きませんが、別ドメインのデータベースと共有していた箇所があった為、データベースの分離も必要でした。
インフラリソースの効率化の為、初期リリース時の判断として共有することとしました。  
こちらもそれ自体は問題ないのですが、レセプト API 内でデータベースの分離予定のテーブルを直接参照している処理がありました。そのテーブルはレセプトの責務外となるので、該当の処理を本来あるべきドメインの API として切り出し直しました。  

法改正プロジェクト開始直後はドメイン分割の影響範囲がわからず、不安がありました。  
ただ API 呼び出しのトレーサビリティを高める対応 ^[リクエストに呼び出し元情報をヘッダ追加したり、アクセス解析ツールを整備したりしました] を入れたり、今回まとめたような責務の再定義をして切り分けができ結果的に良かったと感じています。  
ここまで整理してできていれば、次回の法改正では今回よりも安心して望めると思います。
ただ安心感を持続させるにはドメインの責務を綺麗に保つというのが重要になります。  
今後の改修では責務をより意識して設計を行っていきたいと思います。

## 全体振り返り

前編・後編とで法改正をマイクロサービスで立ち向かうというテーマで書きましたが、如何でしたでしょうか？  
複雑なものを関心事という単位に区切って責務を閉じ込めるということをどのような視点や考え方で行っていっているかが伝われば嬉しく思います。  
関心事をどの視点でどう捉えるかによってサービスの切り口が変わってくるという所がマイクロサービスを設計する上で難しいと感じています。  
今回のケースではその時点では最適な切り口だと思っていたものが、外部環境の変化（今回は法改正）によって関心事が増えてドメイン分割をする判断をしました。  
実際に切り出しする際も将来像を予想しながら行いました。業務のドメイン知識がないと予測も難しいです。

まだ自分自身も手探りで完全に正解とは言い切れないのですが、ただ今回は一定の成果は出せたのではと考えています。    
サービスの責務が明確になり適切に切り分けられ「各チームがそれぞれの関心事に集中して取り組む」ことができたのでは？と思っています。  
法改正の対応では五月雨で上がってくる情報をキャッチアップして各チームがそれぞれ影響範囲を考えてアプリの改修を並行で行う必要があります。  
関心事の分離が出来ておらず責務が曖昧なままだと、安心して並行開発を行うことができません。  
システムが複雑かつ時間的制約があるなかで、集中して取り組む環境がなかったら法改正に立ち向かえなかったと思っています。

## 最後に

明日はいよいよ最後です。

SRE の[@tjinjin](https://qiita.com/tjinjin)です。  
今年 7 本目（！）となる記事を投稿してくれます。お楽しみに！