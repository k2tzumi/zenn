---
title: "アーキテクチャレベルで依存性を逆転させたら最高だった件"
emoji: "🔃"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["アドベントカレンダー","Microservice","DDD"]
published: true
publication_name: "litalico"
published_at: 2024-12-19 00:00
---
:::message
この記事は [LITALICO Advent Calendar 2024](https://qiita.com/advent-calendar/2024/litalico)のシリーズ 1 の 12/19 配信になります。
:::

## はじめに

LITALICO の @katzumi です。
LITALICO に 2020 年に Join してから、ずっとレセプト業務に携わる開発をしてきました。　　
レセプト業務はドメインの複雑さからミスが許されず、さらに３年に一度の大きな報酬改定があり、ロジックが大幅に変わります。  
また、その改訂作業自体が非常に短期間で行う必要があり、年々複雑化するシステムに対応する必要があります。

その複雑な業務に立ち向かった内容を過去にも以下の内容で開発業務の記事を書いていました。

https://zenn.dev/litalico/articles/confronting-law-amends-with-microservices

https://zenn.dev/litalico/articles/confronting-law-amends-with-microservices2

https://zenn.dev/litalico/articles/re-architecting-rezept

https://zenn.dev/litalico/articles/re-architecting-rezept2

今年 2024 年は法改正の年になっており、取り組みの結果その後はどうなったのか？を振り返っていきます。

## 今回も壮絶だった法改正

私自身の大規模法改正の経験が今回で 2 回目となります。
チーム構成としては私ともう一名を除いて前回の　2021 年度法改正を経験したメンバーがいませんでした。
また前回は３種類のサービスのみの対応だったのが、ビジネスの拡大に伴い 3x 種類^[対応範囲が膨大となる為、旧システムでの連携もまだ残っており、法改正時点で実際連携したのは10サービスのみとなりました]のサービスまで拡大する必要がありました。
また、今回の法改正の為に新規で作成した Rezept as a Service で初めて法改正を迎えるという内容でした。
この Rezept as a Servie が各プロダクトと連携を開始した直後での法改正の対応となりました。

今年は補正予算の都合もあり、4 月から２ヶ月間のみに有効になる加算があったり、請求システムを構築する上で必要となる情報が例年よりも数日遅れで公開されるなど大分ばたつきました。
4 月に施行される情報が前年 12 月から五月雨式に公開されていく中ではこの数日はかなりインパクトがある内容となります。
本格的な設計が 2 月から開始し、4 月末にはテストを完了させてリリースしないといけないというスケジュールはやはり痺れるものがあります。
また報酬制度も積み上げで複雑化しており、とあるサービスで利用するサービスコードの数が３倍になっていました。
このサービスコードは請求条件を表しており、このコードの数だけ請求パターンが存在します。純粋に考えれば、前回の３倍のテストが必要です。 ^[全てのパターンでの検証は難しい為、品質を担保する為に工夫はしていてテストケース数が３倍にならない様にしています。それでも複雑度が上がっていることをご理解いただけるのではないでしょうか？]

## システム進化の歴史

最初から Rezept as a Service を作ろうと思って取り組みを行っていたわけではなく、２回の法改正を経て２度のリアーキテクチャを行っています。
以下にコンポーネント図 ^[かなり簡略して書いているので、正確性にかいています。大凡のイメージを掴んで頂ければと思います。] をまとめます。

* 2021 年以前のシステム
    ```mermaid
    graph TD
        subgraph "運営支援システム"
            subgraph "台帳コンポーネント"
                Ledger["台帳<br>データ管理"]
            end
            subgraph "スケジューラコンポーネント"
                Scheduler["スケジューラ<br>実績データ生成"]
            end
            subgraph "レセプトコンポーネント"
                Receipt["レセプト<br>請求書データ作成"]
            end
            Scheduler -- 実績データ確定 --> Receipt
            Receipt -- 総額計算 --> Receipt
            Receipt -- データ取得 --> Ledger
        end
    ```
    レセプトコンポーネントはモノリシックな構成で、お互いの API を呼び合い合うどちらかというと密結合な構成でした。
* 2021 年のリアーキテクチャ（１回目の法改正）
    ```mermaid
    graph LR
        subgraph 運営支援システム
            subgraph レセプトコンポーネント群
                Proxy((Proxy))
                NewReceipt((新レセプトAPI))
                OldReceipt((旧レセプトAPI))
            end
            subgraph その他のコンポーネント
                RequestApp((請求業務アプリ))
                RecordApp((実績登録アプリ))
                LedgerApp((台帳アプリ))
            end
        end
        Proxy -- API呼び出し --> NewReceipt
        Proxy -- API呼び出し --> OldReceipt
        RequestApp -- API呼び出し --> Proxy
        RecordApp -- API呼び出し --> Proxy
        NewReceipt -- データ取得 --> LedgerApp
        OldReceipt -- データ取得 --> LedgerApp
    ```
    時系列で関心事を分離して、振り分けの層をアプリケーションとして実現。請求業務アプリからレセプト業務が独立した状態。
* 2024 年の新アーキテクチャ（２回目の法改正）
    ```mermaid
    graph LR
        subgraph "Rezept as a Service"
            ALB["ALB"]
            RezeptAPI2021((2021年度対応板Rezept API))
            RezeptAPI2024((2024年度対応板Rezept API))
        end
        subgraph "プロダクトA"
            ProductA((請求状態))
        end
        subgraph "プロダクトB"
            ProductB((請求業務))
        end
        subgraph "プロダクトC"
            ProductC((請求状態))
        end
        ProductA -- APIコール --> ALB
        ProductB -- APIコール --> ALB
        ProductC -- APIコール --> ALB
        ALB --> RezeptAPI2021
        ALB --> RezeptAPI2024
    ```
    Rezept as a Service の確立。プロダクトとの責務が明確になり、依存関係が一方通行になった。
    ※Rezept as a Service はマルチテナントなシステムとして構築していますが、データ管理およびシステム構成の都合から、各プロダクトをシングルユースの構成で VPC 上に構築しています。

## コアドメインの蒸留

上述のシステム進化が行われていますが、この構成になるにはコアドメインとなるレセプト業務の本質的な責務の見極めが必要となります。
コア業務以外の周辺業務を切り離していくことを行っていきました。例えば請求業務の中でも利用者への負担額請求は、以前のシステムでは 1 つのマイクロサービスとして定義していましたが、これは明確にレセプトの業務外として定義して分けました。
これはほんの一例ですが、コアドメインの独立性確保する為にいくつか見直しをおこなってきました。
そうすることで、各プロダクト固有の機能に振り回されなくなり、設計の簡素化もできるようになります。

## 依存性を逆転させる

システムの依存関係を考えると、レセプト業務は事業所体制の情報や実績データを必要とします。
本来これらのデータを持つ各コンポーネントの"下流"に位置することになります。
このデータの依存は 2021 年法改正時のシステムでは存在し、上流のデータ仕様が確定するまでレセプトシステムの設計が進められないという問題がありました。
今回この依存の部分を API のインターフェースとして定義することで、このインターフェースに各プロダクトから依存させる形にしました。
これにより、データの流れは従来通り各プロダクトからレセプトシステムへと向かいますが、依存関係は逆転していることになります。

この依存性の逆転により、レセプトシステムは上位モジュールとして振る舞うことができ、各プロダクトの実装の詳細から完全に切り離されることになりました。
例えば、あるプロダクトが実績データの保存方法を変更したとしても、定められたインターフェースを通じてデータを提供する限り、レセプトシステムには一切の影響がありません。

このアプローチは、SOLID の依存関係逆転の原則そのものですが、個々のクラスやモジュールレベルではなく、システムアーキテクチャ全体のレベルでこの原則を適用したという所が、今回の特徴です。これにより、法改正対応という時間的制約の厳しい開発において、レセプトシステムと各プロダクトが並行して開発を進めることが可能となり、プロジェクト全体の開発効率を大きく向上させることができました。

## コアドメインとして依存をなくすことのメリット

コアドメインの見定めと、依存性を逆転させるということはドメイン駆動設計（エヴァンス本）にあるドメインの蒸留になります。
混ざり合ったコンポーネントが分離されています。これは依存させるものを無くし、一番サービスとして安定させる為に行なっています。
最も安定することで、スキーマ駆動開発を通じて Rezept as a Service と各プロダクトが並行して開発できるようになります。
冒頭で触れましたが、法改正は非常にタイトなスケジュールで進行するので手戻りが許されません。
実績データをどの様に蓄積するか？というのは各プロダクトが非常に強い関心をもって作り込みを行なっています。これは使いやすいシステムを実現する為に非常に重要なことです。
ただそうすると、いつまでも実績データの仕様が決まらないことになりがちです。不安定なものに依存するとシステム全体が不安定になります。
手戻りなく迅速に法改正対応をさせ、システム全体を安定ささるには、レセプト業務を一番安定させる必要がありました。

## コミュニケーション設計の重要性

Rezept as a Service に各プロダクトが依存することになることから、重要になってくるのがコミュニケーションになります。
複数チームからの問い合わせによって、コミュニケーションコストの増大が懸念されました。
如何にコミュニケーションコストを少なくできるか？という所が重要です。

### 明確なサービス定義と仕様合意

報酬改訂では、告示内容や留意事項だったり各種資料を読み漁って正しく法解釈し、それを正しくシステムの実装として落とし込みを行っていくことが重要となります。
この法解釈とそれを算定要件としてまとめ、最終的にはシステムの I/F にする感じなのですが、各プロダクトチームメンバーと協業して行っていきました。
この作業が凄くドメイン知識が求められ、また抽象度の高く難易度も高いです。法的根拠の積み上げと、論点の明確化を行っていかないと、議論がループしてしまい、サービスの仕様として合意できません。
幸いなことに、実際に支援事業を行っていたり、社内にドメインのエキスパートがいたり、サービスを利用されている事業所数が多くノウハウの蓄積ができているので、ゴリゴリと決めてけている環境があり大変助けられました。
ただ、関係者が多くなるので、これらの議論を円滑に且つわかりやすくまとめ上げるのに苦労しました。事前にこういうフォーマットで決めていこうとしたものが、機能しなかったり改善すべき点が多かったです。
システム設計であれば Architecture Decision Record（ADR）を残す感じだとは思うのですが、ドメイン特化の算定構造決定レコードをまとめるフォーマットが必要だと考えており、現在試行中です。

### バージョニングされた高品質なドキュメントと整備されたリリースフロー

コミュニケーションコストを最小にして理想な状態としては、連携する際に勝手にドキュメントを見て実装をし、勝手にデプロイして検証・リリースまで持っていってくれることです。
ここで、以下の 3 点が重要になります。

1. ドキュメントとアプリケーションの乖離がないこと  
ドキュメントとアプリケーションの紐づけがされて確認もできること。
2. 明確なバージョンとリリースノートが整備されていること  
そのバージョンと最新版の仕様書にいつでもアクセス可能なこと。
3. バージョン指定可能なリリースフローが整備されていること
実装したバージョンでいつでもセルフサービス型のデプロイができること。

上記のポイントが適切に運用可能な状態にするには、自動化が非常に大事になっています。
これらの自動化をして開発プロセスに乗せるということを実現しました。

詳細を説明すると、今回の記事では収まらないので、今年の３月の [CI/CD Test Night #7](https://testnight.connpass.com/event/311263/) で発表したスライドを紹介しておきます。
良かったら見てください。

@[docswell](https://www.docswell.com/s/katzumi/58G8J9-empowering-release-strategies-cicd-pipelines)

## コアドメインを一番安定させる為に

コアドメインを洗練させたからには一番安定させたいところです。
品質が悪ければ、問い合わが来てコミュニケーションも都度発生して台無しになってしまいます。
開発プロセスとしてスキーマ駆動開発を採用しており、スキーマの品質がプロジェクト全体の成否を左右すると考えていました。

スキーマ駆動を安心して行える様に様々な取り込みを行っていました。

https://zenn.dev/litalico/articles/re-architecting-rezept2

先に触れたリリースフローも含まれていますが、その他にも以下を行っていました。

1. スキーマ自体の品質を向上させる為の取り組み
2. API のシナリオテストの導入

まずは、スキーマ自体の品質を向上させる為の取り組みについては [「実装と乖離させないスキーマ駆動開発フロー / OpenAPI Laravel編」](https://zenn.dev/katzumi/articles/schema-driven-development-flow) でまとめているのでそちらを参照頂ければと思います。
宣伝となりますが、こちらの取り組みの一部を今年の９月に OSS としてライブラリ化を行いました。こちらも良かったらお願いします！

https://zenn.dev/litalico/articles/what-is-eg-r2

次にに API のシナリオテストについてですが、Rezept as a Service には画面が存在せず、 API しか存在しないアプリケーションになるのでどの様にテストしていくか？はプロジェクトの成功を左右する重要な要素でした。
API シナリオテストについて色々調べていた所、 [runn](https://github.com/k1LoW/runn) に出会うことでき、惚れ込んでプロジェクトに導入して今では無くてはならない存在になっています。
runn については以下の記事を参考にしてください。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

興味を持たれた方は去年一人アドベントカレンダーした記事を取りまとめたチュートリアルを本を作りましたので良かったらどうぞ。

https://zenn.dev/katzumi/books/runn-tutorial

## なにが最高なのか？

ここまで、コアドメインを見定めし、ドメインの蒸溜についてお話しました。また、よりコアドメインに集中する際の勘所についても触れさせて頂きました。
コアドメインが他に依存するものがなく、安定した仕様とすることでスキーマ駆動開発を円滑に行うということは、狙い通り実現できたかと思います。

戦略としてコアドメインを蒸溜させる判断をしましßたが、実際にコアドメインに向き合ってみると思った以上に良かったと感じています。

1. 責務がより明確になり、関心事にのみ集中すれば良くなる
以前だったら、実績データの仕様の動向を気にかけていましたが、今回は自身の持つ I/F のみを気にすれば良くなりました。
2. I/F 仕様書に向き合ってテストできる
１にも通じますが、依存していた実績データのパターンは考える必要はなく、I/F が契約プログラミングとなります。厳密なパターンでのテストケースを作成すれば良くなりました。
これによってインターフェースの品質が十分高い状態であれば、後は単体テストに集中すればシステム全体の品質を上げることができるようになります。
3. コミュニケーションコストを最低限に抑えコアドメインに集中できる
コアが安定していれば、独立性を保って開発を進められる為、逆に問い合わせが少なくなります。
4. システム全体の開発のアジリティ向上できる
スキーマ駆動開発を採用している理由でもあったりしますが、コアドメインの仕様が先に決まることで全体の開発サイクルが早く回ります。

これらのメリットは、法改正の対応時に非常にありがたかったです。
以前は、データの下流ということもあり、一番テストケースが多いのに開発の終盤にテストが集中してしまって時間が足りませんでした。
これが、連携テスト開始までに一定水準以上のテストができるようになりました。

## 2度のリアーキテクチャを振り返る

2021 年と 2024 年の 2 度にわたるリアーキテクチャを行ってきましたが、最初から現在のアーキテクチャにすることは中々難しかったと思います。
システムの理解度も高くなかったですし、早すぎる最適化を行うことで独りよがりで使いづらいシステムになっていた可能性もあると考えています。
継続的に改善の取り組みをしてきて以下のことが実現できたかと考えています。

1. コンポーネント間の依存関係を最小化
2. Rezept as a Service を中心とした設計
3. インターフェースによる明確な契約の定義
4. スキーマ駆動開発の本格的な導入

これらの変更により、システムの拡張性、保守性、そして法改正への対応力を大幅に向上させることができたと実感しています。

# 最後に

長文になりましたが、如何だったでしょうか？
設計原則のアーキテクチャレベルでの適用効果がご理解頂けたら幸いです。
今後も引き続き、コアドメインの洗練をしていきます。

:::message
明日は、[LITALICO Advent Calendar 2024](https://qiita.com/advent-calendar/2024/litalico) 20 日目の記事です。[@enochan](https://qiita.com/enochan) さん、　[@bashiiko](https://qiita.com/bashiiko)さん、[@ryunosuke_sako](https://qiita.com/ryunosuke_sako)と[@ayami_n](https://qiita.com/ayami_n)さんが担当し、豪華４本立てになります！どうぞお楽しみに！
:::