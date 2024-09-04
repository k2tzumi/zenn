---
title: "頑張らないスキーマ駆動開発を支える『eg-r2』を公開しました"
emoji: "🦥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["openapi","laravel","PHP","スキーマ駆動開発"]
published: true
---

こんにちは。 [@katzumi](https://zenn.dev/katzumi) です。  
LITALICO でレセプト基盤グループのマネージャーを務めています。

今回の記事では、スキーマ駆動開発を強力にサポートするライブラリ『eg-r2』を開発して OSS として公開しましたので、紹介します。
このライブラリを利用して、スキーマ駆動開発における API 仕様書とソースコードの 2 重管理を解消できました。

## Rezept as a Service の共通モジュールとして生まれた

まず、今回のライブラリのベースを開発したプロジェクトについて説明します。
LITALICO では社内外で利用される複数のプロダクトを開発しています。
これらのプロダクトのコア業務であるレセプト業務を X as a Service として開発しました。

以前、こちらのプロジェクトの趣旨及び立ち上げの内容をアドベントカレンダーで記事にしました。

https://zenn.dev/katzumi/articles/re-architecting-rezept

こちらのプロジェクトではスキーマ駆動開発を採用しており、円滑かつ高品質に開発できる仕組みが必要だと考えました。

## スキーマ駆動開発の課題

上記の記事でも触れていますが、OpenAPI V3 を利用してのスキーマ駆動開発のプロセスを採用しました。
プロジェクトの特性として以下がありました。

* API 数が多数にのぼり、スキーマ自体が複雑となる　^[現時点で200弱、3年で更に100程度増える見込み。リクエストパラメータ数は50-200程度]
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

OpenAPI で仕様書を記述する開発プロセスには、以下の 3 つのパターンがあります。

1. API 仕様書とソースコードを独立して記述する手法
2. API 仕様書からコードを自動生成する手法
3. ソースコードから API 仕様書を生成する手法

1 は王道的で馴染みのある手法ではあるものの、API 仕様書と実装が乖離してしまう可能性が高いと考えていました。
API 仕様書を非テキスト形式で記述すると、GitHub でのレビューが難しく、複数のツールを参照しながら開発するのは手間がかかります。
また、スキーマが複雑でリクエストパラメータも多い場合、レビューが困難です。

今回は、2 または 3 のアプローチを取り入れ、自動化することでソースコードと API 仕様書の乖離を防ごうとしました。
しかし、API 仕様書を先に書くかソースコードを先に書くかで悩んでいました。

## 表現力は フレームワークを通して具象化されるAPI > OpenAPI となる

OpenAPI のエコシステムは充実しており、API 仕様書からコードを自動生成したり、ソースコードから API 仕様書を生成するライブラリがいくつか存在します。  
しかし、これらのライブラリはアウトプットが一方通行であればうまく機能しますが、相互変換ができないため、変更の流れを一方通行にすることが重要です。
変更の流れを一方通行にする上で注目したいのが、API に対しての仕様の表現力になります。
昨今の REST API の開発では何かしらのフレームワークを利用するのが大半です。
そのフレームワークを通じて実装される API の振る舞いと OpenAPI の仕様上のギャップが生まれます。
ソースコードから API 仕様書を自動生成するのは、OpenAPI の表現力が足りず、フレームワークのバージョンアップに追従する必要があるため、適切ではないと考えました。

## API仕様を構造化したメタデータとしてコードの宣言時に埋め込む swagger-php

OpenAPI 仕様書を記述する方法として [swagger-php](https://github.com/zircote/swagger-php) というライブラリがあり、PHP のソースコード上に Attributes で API 仕様書を書くことができます。

```php
    #[OA\Get(path: '/api/users')]
    #[OA\Response(response: 200, description: 'AOK')]
    #[OA\Response(response: 401, description: 'Not allowed')]
    public function users() { /* ... */ }
```

API 仕様書をソースコード上に記述することで、GitHub レビュー時に便利です。
実装のコードと仕様を近い場所に記述できるため、書きやすく読みやすいのでオススメです。
Attribute を使用することで、OpenAPI の文章構造がクラスとして表現され、IDE の入力支援も受けられるため、安心して記述できます。
swagger-php はメタデータを API 仕様書として出力します。しかし、このメタデータはドキュメント生成時にのみ利用されており、勿体ないと感じました。そこで、便利機能を実装したのが今回紹介する eg-r2 というライブラリです。

## eg-r2 の紹介

今回紹介するライブラリが eg-r2 になります。

https://github.com/litalico-engineering/eg-r2

名前の由来は、2 つの R（リクエストの検証とルーティング）を Easy（イージー）に Generate するという意味で、音の響きと頭文字 eg のダブルミーニングです。

> Easy request validation and route generation from open API specifications

eg-r2 では swagger-php 経由で記述された OpenAPI の仕様を読み込んで、以下を行います。

1. リクエストのバリデーションの自動化
2. ルーティング生成の自動化

OpenAPI 仕様書を読み込み、リクエストの JSON バリデーションを行うライブラリはいくつかあります。ただ、テスト用のライブラリとしては使えますが、プロダクションコードとしてはパフォーマンス的に厳しいと感じました。
今回のプロジェクトでは、OpenAPI 仕様が大きく、JSON ファイルのサイズが約 2MB あるため、リクエストごとにファイルを読み込むのはパフォーマンス的に厳しいと考えました。　
ファイルベースではなく Attributes を使えば、Reflection 経由で関連する仕様のみを読み取り、高速に動作できます。
次にルーティングはエンドポイントが複数コントローラークラスにまたがるため、Reflection では処理が重くなると予測し、コード生成する方針を採用しました。  
ルーティングの更新頻度はリクエストパラメータより少なく、後続の開発でも修正が不要なので、コード生成でも問題ないと考えました。

## 前提条件

* PHP8.2 以上  
Attribute を使う前提となるので 8.1 以上必要です。Active support である 8.2 以上としています。
* swagge-php  
* Laravel10 or Laravel11  
Laravel10 は version [0.0.6](https://github.com/litalico-engineering/eg-r2/releases/tag/0.0.6) まで。
version [1.0.0](https://github.com/litalico-engineering/eg-r2/releases/tag/1.0.0) 以上は Laravel11 向けとなります。

## [eg-r2 の導入方法](https://github.com/litalico-engineering/eg-r2?tab=readme-ov-file#installation)

eg-r2 をプロジェクトに導入する方法について説明します。以下の手順に従ってください。

1. **インストール**
   ```bash
   composer require litalico-engineering/eg-r2
   ```
2. 設定 config ディレクトリに eg-r2.php ファイルを作成し、必要な設定をする。
   ```bash
   php artisan vendor:publish --provider="Litalico\EgR2\Providers\GenerateRouteServiceProvider
   ```

Laravel 標準のディレクトリ構成であれば、上記手順のみで OK です。
別途 Controller の namespace が存在する場合は、 `config/eg-r2.php` の `namespaces` に追記してください。

## 機能紹介

具体的な使い方についてはサンプルプロジェクトを用意しましたのでそちらも参考にしてみてください。

https://github.com/k2tzumi/eg-r2-example

API 仕様書を作成するコマンドも make で用意しています。API 仕様書と実際のソースを比較して見ることをオススメします。

```bash
make redoc
```

### リクエストのバリデーション自動生成方法

eg-r2 を使用すると、リクエストのバリデーションを自動生成できます。
これにより、手動でバリデーションルールを記述する手間が省け、コードの一貫性が保たれます。

### 手順

1. **リクエストクラスの作成**
  まず、リクエストクラスを作成します。
  FormRequest に 2 つの Trait を use してください。

  https://github.com/k2tzumi/eg-r2-example/blob/main/app/Http/Requests/Pet.php#L22-L36

  まず `Litalico\EgR2\Http\Requests\RequestRuleGeneratorTrait` について説明します。
  この Trait は　swagger-php の Attribute を読み込んで Laravel 向けの Validator の定義を自動展開します。
  rules メソッドが上書きされて、バリデーションルールを改めて実装する必要がなくなります。

OpenAPI で定義されている型（`type`）や制限（`pattern`）と制約等（`required` や `nullable`）が、Laravel のバリデーションルールに変換されます。
こちらの変換ルールですが、OpenAPI で扱える記述をいい感じに変換されるように実装しています。

OpenAPI の定義では表現が弱く、どうしても Laravel 標準または独自定義したバリデーションを使いたいケースもあります。
その場合、以下の様に x プロパティを使って変更ができます。

https://github.com/k2tzumi/eg-r2-example/blob/main/app/Http/Requests/Order.php#L53-L62

上記の様にすると正規表現だけでは厳密にチェックできない日時等のチェックを Laravel の `date_format` 指定によるチェックが出来るようになります。
object 内の key のユニーク制約を担保させたい場合も以下の様に記述できます。

```php
#[Poperty('key', nullable: true, x: ['validation' => ['distinct']]]
public string $key;
```

2. **リクエストオブジェクトへのアクセス方法**
  もう 1 つの `Litalico\EgR2\Http\Requests\FormRequestPropertyHandlerTrait` の説明をします。  
  こちらを use すると、バリデーションが OK になったら、そのパラメータを FormRequest クラスのプロパティとしてアクセスできます。
  通常の FormRequest クラスのパラメータのプロパティへのアクセスはマジックメソッド経由になっている為、直接プロパティを定義できません。
  その問題を回避して IDE 補完をさせる為には以下のような記述をしたりします。
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
    #[Schema(title: 'My request', required:['age', 'name', 'is_active'])]
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

### 手順

1. **コントローラークラスの作成**
  まず、コントローラーを作成します。
  
https://github.com/k2tzumi/eg-r2-example/blob/main/app/Http/Controllers/Pet.php#L14-L46

  通常のコントローラークラスにエンドポイントのメソッドを定義します。
  そのメソッドに対して swagger-php でエンドポイントの仕様を記載します。

2. **ルーティングファイルの作成**
  アプリケーションのルーティング設定のルートファイルは以下のコマンドで自動生成がされます。
    ```bash
    php artisan eg-r2:generate-route
    ```
    実際に自動生成されたファイルがこちらになります。

  https://github.com/k2tzumi/eg-r2-example/blob/main/routes/eg_r2.php

  パスを変更したい場合は、 `config/eg_r2.php` の [route_path](https://github.com/k2tzumi/eg-r2-example/blob/main/config/eg_r2.php#L26) を編集してください。

## 取り組み結果・感想

1 年半前 ^[eg-r2 を OSS 化する以前から取り組みを行っています] から今回紹介した開発プロセスでスキーマ駆動開発を行ってきた感想を以下にまとめます。

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

サンプルプロジェクトでは、動作を検証する為に敢えて Feature テストでリクエストのバリデーションのテストケースを実装しましたが、不要と判断しテストは書かない方針としました。

eg-r2 を使うと、API 仕様書が完成した時点でリクエスト受付可能なエンドポイントが用意されるため、必然的にスキーマ駆動開発レディとなります。

200 弱のエンドポイントを実装してきましたが、OpenAPI の表現力でほばカバーできました。導入によってバリデーションやルーティングが書きづらくなったということはありませんでした。  
eg-r2 の自動生成機能は非常に便利ですが、API 仕様で対応できないケースがあると、思わぬ嵌りが発生するのではないかと心配する人も多いでしょう。
しかし、最悪の場合でもルートファイルは書き換え可能ですし、バリデーションの自動生成は Trait として機能提供しています。必要に応じてルールを部分的（なんなら全て）で書き換え可能であるという安心感がありました。

## まとめ

本記事では、eg-r2 ライブラリを用いた API 仕様書の自動生成方法について解説しました。
以下に主要なポイントをまとめます。

1. **導入のハードルが低い**:
   - swagger-php を利用していれば、ひと手間で eg-r2 を使い始められる。
   - 自動生成している部分は Laravel 仕様に準拠しているため、いつでも使い捨てできる。

2. **自動生成の利便性**:
   - 動くコードを書くだけで高品質な API 仕様書が自動的に生成されるため、手間をかけずに API 仕様書を作成できる。
   - swagger-php を利用することで、API 仕様書もコードレビューの対象とでき、品質向上に寄与する。

3. **スキーマ駆動開発のサポート**:
   - API 仕様書が完成した時点でリクエスト受付可能なエンドポイントが準備されるため、別途モックサーバーを用意する必要がありません。
   - スキーマ駆動開発に対応しており、API 仕様書と実装が常に同期しているため、開発効率が向上する。

4. **OpenAPI 仕様の記述形式に合わせる**:
   - OpenAPI 仕様の記述形式に合わせることで、自動生成の力が最大限発揮され、仕様変更時でも円滑な開発ができる。

5. **柔軟なカスタマイズ**:
   - ルートファイルやバリデーションの自動生成は、必要に応じて部分的または全て書き換えることが可能。これにより、プロジェクトの要件に柔軟に対応できる。

eg-r2 を活用することで、効率的かつ高品質な API 開発が可能となります。

## 最後に

eg-r2 というライブラリについてデザイン設計も含めて紹介させて頂きました。
今回は Laravel 向けのライブリとなりますが、設計思想は他のフレームワークでも有効と考えています。

次のステップとして、eg-r2 の導入事例を増やし、OpenAPI の仕様のカバー範囲と Laravel での再現性の向上を目指したいと考えています。
また、より良い開発者体験となるように改善を続けていきます。

そのために、読者の皆様には実際に eg-r2 を導入していただき、フィードバックを頂けると幸いです。
皆様のご意見やご感想を頂けますと大変うれしいです。

今回紹介したライブラリも含めたスキーマ駆動開発フローを記事にまとめています。
よりスキーマの品質を上げるためのプロセスに言及していますので、ぜひ合わせてご覧ください。

https://zenn.dev/katzumi/articles/schema-driven-development-flow
