---
title: "PHPUnitでカバレッジがうまく取れなくて嵌った話"
emoji: "🧪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["php","phpunit","coverage","やらかし"]
published: false
---

## はじめに

以前、こんな記事を書いていました  
https://zenn.dev/katzumi/articles/995df5abebc91e312167

こちらの時とは別のプロジェクトでカバレッジがうまく取れなくて嵌ったお話をします。

## やけにカバレッジ低くない？

新規プロジェクトだったということもあり、初期段階でLaravel9から使える様になったテストカバレジオプションで雑にカバレッジを見ていました

```console
$ php artisan test --coverage
```

開発サイクルも回り始めて、いよいよちゃんとカバレッジを見れるようにと上記の様にカバレッジをPRコメントで見えるようにしました。  
そこで異変に気づきました。やけにカバレッジが低いぞと。。

## 原因で考えられたこと

新しいプロジェクトなので微妙にバージョンが違います

* PHPのバージョン違い
* PHPUnitのバージョン違い  
レポートの指定も変わりました  
https://phpunit.readthedocs.io/en/9.5/configuration.html#the-report-element  
カバレッジ取得後に別途レポート出力用のコマンドを実行しなくても良くなりました。  
phpunit.xmlに設定するだけでレポート出力までやってくれて便利!
* 拡張モジュールの違い  
pcovからXDebug

色々変更点がおおく、調査も難航しました。  
同僚がまず先行して調べてくれていましたが、お手上げで自分も色々設定まわりやらなにんやら調べて見てもわからん！となりました。。

## 原因だったもの

自分が原因を作り込んでいました orz  
今回のプロジェクトからPHPUnitの書き方で一つ変えてた所があり、 `アノテーションを入れてテストコードの追跡性を良くしましょう` というのがありました。  

[@covers](https://phpunit.readthedocs.io/ja/latest/annotations.html#covers) です  

![@coversの説明]](/images/articles/apologies-for-phpunit-coverage/description_of_covers.png)

これを使うとテストの目的が明示されるので良いのと、なによりIDEのサポートが得られプロダクションコードからテストコードにjumpさせることができます。  
jumpできない場合はテストコードが書いてないことがわかり、便利だなーと思いコツコツとアノテーションを埋め込んでいきました。　　  

`@covers`アノテーションの書き方はいくつかあり、以下の表がまとめられていました

![@coversの推奨記述形式]](/images/articles/apologies-for-phpunit-coverage/recommended_description_format.png)

* `@covers ClassName`
* `@covers ::functionName`

のみが推奨されているようです。  
後者は `@coversDefaultClass` と組み合わせて使います。  

https://phpunit.readthedocs.io/ja/latest/annotations.html#coversdefaultclass
![@coversDefaultClassの説明]](/images/articles/apologies-for-phpunit-coverage/description_of_covers_default_class.png)


実際に上記の図のサンプルコードの様に記載していきました。  
こちらのアノテーションが起因していました。

## 見落としていたもの

調べていて初めて気づきました orz

> @covers アノテーションが利用された場合、出力されるコードカバレッジの算出方法が変わります。 アノテーションで自身を指定するテストから実行された場合のみ、「covered」と判定されます。 アノテーションで自身を指定していないテストから間接的に利用されたとしても、「covered」とは判定されません。 これによって、コードカバレッジが実際よりも高く出る false positive を避けることができます。

確かにテストコード内で直接呼び出しを行っているコード。。特にprivateメソッドは全てカバレッジが取得されていませんでした。  

## 対応したこと

* テストクラスのdocblokに `@coversDefaultClass` に指定したクラスを `@covers` にも指定する
* 上記とあわせて、テスト対象クラスが依存しているファイルもあわせて `@covers` に追加する  
単体テストが書かれているものは記載せず、単体テストが難しい ^[敢えて割愛しているともいう。。] クラス。。例えばEloquentモデルとかを追加
* PHPUnitの設定に `forceCoversAnnotation="true"` を追加  
これを入れておくとテスト実行時にアノテーションが未指定の場合にWARNとなります

## やらなかったこと

> このアノテーションはテストクラスの docblock に記載することも、個別のテストメソッドの docblock に記載することもできますが、 テストクラスの docblock に記載することが推奨され、個別のテストメソッドの docblock に記載することは推奨されません。

`@covers` を個別のテストメソッドの docblock に記載することは推奨されないっぽいですが、上述のIDEの恩恵にあやかりたく、直接テストメソッドまでjumpさせたかったのでこちらの推奨は守りませんでした

## 所管

`@coversDefaultClass` をテストクラスの docblock に記載したら、その対象クラスが全て全てcoverage対象になってくれれば良いのに。。と思いつつも

> コードカバレッジが実際よりも高く出る false positive を避けることができます。

こちらを優先されたんだろうなーと。  
厳密にテスト対象を把握させろ（しろ）という意思を感じました

## この記事で伝えたかったこと

:::message
ドキュメントをしっかり読め！
:::

ですハイ。  
肝に命じます。。   
あと `@covers` の指定はこうやって書くんだよーというのがあれば知りたいです。  
