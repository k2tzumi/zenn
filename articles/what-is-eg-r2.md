---
title: "頑張らないスキーマ駆動開発を支える『eg-r2』を公開しました"
emoji: "🦥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["openapi","laravel","PHP","スキーマ駆動開発"]
published: false
---

こんにちは。LITALICO の [@katzumi](https://zenn.dev/katzumi) です。  
レセプト基盤グループというチームで請求プラットフォームを作成しています。

最近、 eSports の大会の観戦にハマっています。パブリックビューイングできるオススメな飲食店があれば是非教えてください。

さて今回の記事ではスキーマ駆動開発を強力にサポートするライブラリを開発して OSS として公開しましたので、紹介させて頂きます。
このライブラリを利用して、スキーマ駆動開発における API 仕様書とソースコードの 2 重管理をなくすことができました。

## Rezept as a Service

まず今回のお話するライブラリのベースを開発したプロジェクトの説明をさせてください。
LITALICO が社内外で利用・開発しているプロダクトが複数あります。
そのプロダクトのコア業務であるレセプト業務を X as a Service として開発しました。

以前、こちらのプロジェクトの趣旨及び立ち上げの内容をアドベントカレンダーで記事にしました。

https://zenn.dev/katzumi/articles/re-architecting-rezept


こちらのプロジェクトでスキーマ駆動開発を採用しており、円滑かつ高品質に開発できる仕組みが必要だと考えました。

## 開発目的

上記の記事でも触れていますが、スキーマ駆動開発で OpenAPI V3 のフォーマットを採用しました。
プロジェクトの特性として以下がありました。

* API 数が多数にのぼり、スキーマ自体が複雑となる
* 非常に限られた最短時間で API 仕様書のスキーマを公開して複数チームで並行開発する必要がある
* 不確実性が高く、スキーマ公開後に I/F を調整するリスクがある
* 3 年に 1 回、8 割の API が更新対象となる
* 新バージョンの API は差分更新する

高品質な API 開発を短期間で行える必要がありました。
スキーマ駆動開発で API の仕様書と実装が乖離してしまう問題を防ぎたいと考えました。

## API仕様書と実装を絶対乖離させない

スキーマ駆動開発で手戻りが発生することは絶対さけたいと考えていました。
また、API 仕様書の公開を迅速に行う必要があり、仕様書の作成と実装を繰り返す可能性があります。
その中で、API 仕様書と実装を乖離させないというのは、戦略が必要だと考えました。

* そもそもどうやって API 仕様書を書くのか？
* API 仕様書というアウトプットが開発プロセスの中のどういう位置づけにするか？
* 仕様書のレビューをどうするか？
* etc..

スキーマ駆動開発に関わる上記のようなアレコレについて悩みが発生します。

## コードが先か仕様書が先か？

OpenAPI でのスキーマ駆動開発には、以下の 3 つのパターンがあります。

1. API 仕様書とソースコードを独立して記述する手法
2. API 仕様書からコードを自動生成する手法
3. ソースコードから API 仕様書を生成する手法

1 は従来通りの開発で、API 仕様書と実装が乖離してしまう可能性は高いと考えていました。
API 仕様書を非テキスト形式で記述すると、GitHub でのレビューが難しく、複数のツールを参照しながら開発するのは手間がかかります。
また、スキーマが複雑でリクエストパラメータも多い場合、レビューが困難です。

今回は、2 または 3 のアプローチを取り入れ、自動化することでソースコードと API 仕様書の乖離を防ごうとしました。
しかし、API 仕様書を先に書くかソースコードを先に書くかで悩みました。

## 表現力は フレームワークを通して具象化されるAPI > OpenAPI となる

OpenAPI のエコシステムは充実しており、API 仕様書からコードを自動生成したり、ソースコードから API 仕様書を生成するライブラリがいくつか存在します。  
しかし、これらのライブラリはアウトプットが一方通行であればうまく機能しますが、相互変換ができないため、変更の流れを一方通行にすることが重要です。
変更の流れを一方通行にする上で注目したいのが、API の仕様に対する表現力になります。
昨今の REST API の開発では何かしらのフレームワークを利用するのが大半です。
そのフレームワークを通じて実装される API とその仕様が OpenAPI の仕様書との表現力のギャップがあります。
ソースコードから API 仕様書を自動生成するのは、OpenAPI の表現力が足りず、フレームワークのバージョンアップに追従する必要があるため、適切ではないと考えました。

## API仕様を構造化したメタデータとしてコードの宣言時に埋め込む swagger-php

OpenAPI 仕様書を記述する方法として [swagger-php](https://github.com/zircote/swagger-php) というライブラリがあり、PHP のソースコード上に Attributes で API 仕様書を書くことができます。

```php
    #[OA\Get(path: '/api/users')]
    #[OA\Response(response: 200, description: 'AOK')]
    #[OA\Response(response: 401, description: 'Not allowed')]
    public function users() { /* ... */ }
```

API 仕様書をソースコード上に書けて、GitHub レビュー時に便利です。
実装のコードと仕様が近い場所で書けるので書きやすいし、読みやすいのでオススメです。
Attribute なので OpenAPI の文章構造もクラスとして表現されており、IDE の入力支援もされるので安心して書けます。
swagger-php はメタデータを API 仕様書として出力します。ただこのメタデータはドキュメント生成時にのみ利用されていて、勿体ないと思いついて便利機能を実装したのが、今回紹介する eg-r2 というライブラリとなります。

## eg-r2 とは？

今回紹介するライブラリが [eg-r2](https://github.com/litalico-engineering/eg-r2) になります。
名前の由来は 2 つの R（リクエストの検証とルーティング）を Easy（イージー）に Generate する（音の響きと頭文字 eg のダブルミーニング）という意味となります。

> Easy request validation and route generation from open API specifications

eg-r2 では swagger-php 経由で記述された OpenAPI の仕様を読み込んで、以下を行います。

1. リクエストのバリデーションの自動化
2. ルーティング生成の自動化

OpenAPI 仕様書を読み込み、リクエストの JSON バリデーションを行うライブラリはいくつかあります。しかし、テスト用のライブラリとしては使えますが、プロダクションコードとしてはパフォーマンス的に厳しいと感じました。
今回のプロジェクトでは、OpenAPI 仕様が大きく、JSON ファイルのサイズが約 2MB あるため、リクエストごとにファイルを読み込むのはパフォーマンス的に厳しいです。　
Attributes を使えば、Reflection 経由で関連する仕様のみを読み取り、高速に動作できます。
ルーティングはエンドポイントが複数コントローラークラスにまたがるため、Reflection では処理が重くなると予測し、コード生成する方針を採用しました。  
ルーティングの更新頻度はリクエストパラメータの変更より少なく、後続の開発でも修正が不要と判断したため、コード生成でも問題ないと考えました。

## 前提条件

* PHP8.2 以上  
Attribute を使う前提となるので 8.1 以上必要です。Active support である 8.2 以上としています。
* swagge-php  
* Laravel10 or Laravel11  
Laravel10 は version [0.0.6](https://github.com/litalico-engineering/eg-r2/releases/tag/0.0.6) まで。
version [1.0.0](https://github.com/litalico-engineering/eg-r2/releases/tag/1.0.0) 以上は Laravel11 向けとなります。

## [セットアップ方法](https://github.com/litalico-engineering/eg-r2?tab=readme-ov-file#installation)

まず composer install します。

```
composer require litalico-engineering/eg-r2
```

次に vendor パブリッシュしてください。

```
php artisan vendor:publish --provider="Litalico\EgR2\Providers\GenerateRouteServiceProvider
```

Laravel 標準のディレクトリ構成であれば、上記手順のみで OK です。
別途 Controller の namespace が存在する場合は、 `config/eg-r2.php` の `namespaces` に追記してください。

## 機能紹介

具体的な使い方については [サンプルプロジェクト](https://github.com/k2tzumi/eg-r2-example) を用意しましたのでそちらも参考にしてみてください。
API 仕様書を作成するコマンドも make で用意していますので、API 仕様書を見ながらソースを眺めてもらえると🙏。

```
make redoc
```

### リクエストのValidationの自動生成方法

FormRequest に 2 つの Trait を use してください。

https://github.com/k2tzumi/eg-r2-example/blob/main/app/Http/Requests/Pet.php#L22-L36

まず `Litalico\EgR2\Http\Requests\RequestRuleGeneratorTrait` について説明します。
この Trait は　swagger-php の Attribute を読み込んで Laravel 向けの Validator の定義を自動展開します。
rules メソッドが上書きされて、Validation ルールを改めて実装する必要がなくなります。

OpenAPI で定義されている型（`type`）や制限（`pattern`）と制約等（`required` や `nullable`）が、Laravel の Validation ルールへ変換されます。
こちらの変換ルールですが、OpenAPI で扱える記述をいい感じに変換されるように実装しています。

OpenAPI の定義では表現が弱く、どうしても Laravel 標準または独自定義した Validation を使いたいケースもあります。
その場合、以下の様に x プロパティを使って拡張することが出来るようにしています。

https://github.com/k2tzumi/eg-r2-example/blob/main/app/Http/Requests/Order.php#L53-L62

上記の様にすると正規表現だけでは厳密にチェックできない日時等のチェックを Laravel の `date_format` 指定によるチェックが出来るようになります。
object 内の key のユニーク制約を担保させたい場合も以下の様に記載できます。

```php
#[Poperty('key', nullable: true, x: ['validation' => ['distinct']]]
public string $key;
```

もう 1 つの `Litalico\EgR2\Http\Requests\FormRequestPropertyHandlerTrait` の説明をします。  
こちらを use すると、Validation が OK になったら、そのパラメータを FormRequest クラスのプロパティとしてアクセスできるようにしています。
通常の FormRequest クラスのパラメータのプロパティへのアクセスはマジックメソッド経由になっている為、直接プロパティを定義できません。
IDE 補完をさせる為に以下のような記述します。

```php
/**
 * @property int $age
 * @property string $name
 * @property bool $is_active
 */
class MyFormRequest extends FormRequest
{
  public function rules()
  {
    return [
      'age' => 'required|integer',
      'name' => 'required|string',
      'is_active' => 'required|boolean',
    ];
  }
}
```

eg-r2 の FormRequest では、直接プロパティ定義がされます。これにより、Controller から真に型安全で処理が書けるようになります。　^[アノテーションで記載する方法では静的解析での型チェックはできますが、実行時の型チェックまではできません。]

```php
#[Schema(title: 'My request', required: ['age', 'name', 'is_active'])]
class MyFormRequest extends FormRequest
{
  use RequestRuleGeneratorTrait, FormRequestPropertyHandlerTrait;  

  #[Property(property: 'age', type: 'integer', format: 'int64')]
  public int $age;

  #[Property(property: 'name', type: 'string')]
  public string $name;

  #[Property(property: 'is_active', type: 'boolean')]
  public boolean $is_active;

  // roulesメソッドはtraitで自動生成しているので不要
}
```

上記 FormRequest を定義して Controler からは以下の様に記載できます。

```php
  public function something(MyFormRequest $request): JsonResponse
  {
    // ageを参照する処理
    $request->age,
    // .. snip  ..
```

### ルートファイルの自動生成方法

アプリケーションのルーティング設定のルートファイルは以下のコマンドで自動生成がされます。

```
php artisan eg-r2:generate-route
```

実際に自動生成されたファイルがこちらになります。

https://github.com/k2tzumi/eg-r2-example/blob/main/routes/eg_r2.php

もしパスを標準の `routes/eg_r2.php` から変更したい場合は、 `config/eg_r2.php` の [route_path](https://github.com/k2tzumi/eg-r2-example/blob/main/config/eg_r2.php#L26) を編集してください。


## やってみてどうだったか？

`eg-r2` を OSS 化する前の 1 年半前から同じ開発プロセスでスキーマ駆動開発を行ってきた感想を以下にまとめます。

* API 仕様書と実装の乖離を心配する必要がなくなった
* 開発者体験がすこぶる良い
  * 手数が減る
  * 型安全に書ける
  * API 仕様もコードとして GitHub 上でレビューできる
* モックサーバーが不要で最速でエンドポイント公開できる
* 自然とスキーマ駆動開発になる
* 案外 OpenAPI のスキーマ定義の表現力で事足りる

今回の取組としては、API 仕様書と実装の乖離を防ぎたいというモチベーションでした。
API 仕様書の品質を高めたいという思いでしたが、品質だけでなく機敏性や生産性でもメリットがあると感じています。

使い心地としては、API 仕様書を書くというよりも、動くコードを書いていたら高品質な API 仕様書が自動的に生成される感じです。  
swagger-php を使うことで、API 仕様書もコードレビューができる点は非常に優れています。  
プロジェクトによっては API 仕様書をまとめる時間がなく、書かれていないケースも多いですが、これなら手間をかけずに API 仕様書をまとめることができます。

当初はスキーマ駆動開発を行う想定で、OpenAPI 仕様書からモックサーバーを立てることも検討していました。 ^[[Prism](https://github.com/stoplightio/prism)とかがあります]
しかし、今回の仕組みでは API 仕様書が完成した時点でリクエスト受付可能なエンドポイントが準備されるため、別途モックサーバーを用意する必要がなくなりました。^[正常リクエストを送信すると空レスポンスが返却される。仮実装すればダミーレスポンスを返却できます]

サンプルプロジェクトでは、動作を検証する為に敢えて Feature テストでリクエストの Validation のテストケースを実装しましたが、今のプロジェクトではテストは書かないプロセスとしました。

eg-r2 を使うと、API 仕様書が完成した時点でリクエスト受付可能なエンドポイントが用意されるため、必然的にスキーマ駆動開発レディとなります。

100 以上のエンドポイントを実装してきましたが、API 仕様書のスキーマ定義の表現力で validation やルーティングが不便になったということはありませんでした。  
eg-r2 の自動生成機能は非常に便利ですが、API 仕様で対応できないケースがあると、思わぬ嵌りが発生するのではないかと心配する人も多いでしょう。
しかし最悪の場合でもルートファイルは書き換え可能ですし、Validation の自動生成は trait として機能提供していますのでルールを部分的（なんなら全て）で書き換え可能であるという安心感がありました。

## 最後に

eg-r2 というライブラリについてデザイン設計も含めて紹介させて頂きました。
今回は Laravel 向けのライブリとなりますが、設計思想は他のフレームワークでも有効と考えています。
ぜひご意見やご感想をお聞かせください。

今回紹介したライブラリも含めたスキーマ駆動開発フローを記事にまとめています。よりスキーマの品質を上げる為のプロセスに言及していますので、合わせて御覧ください。

https://zenn.dev/katzumi/articles/schema-driven-development-flow
