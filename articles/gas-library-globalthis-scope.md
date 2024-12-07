---
title: "アプリ内のfunctionをGASライブラリからコールバックさせる方法"
emoji: "📞"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["gas"]
published: true
---

## はじめに

以前こんな GAS ライブラリを実装していたのですが、その当時では表題の件をうまく実装できていませんでした。

https://zenn.dev/katzumi/articles/58354fb4d05038

かなりニッチなケースだとは思いますが、記事にしたいと思います。

## やりたかったこと

https://github.com/k2tzumi/apps-script-jobqueue

こちらの GAS ライブラリは GAS で非同期処理を実現するものです。
以前の処理フローはこちらの様になっていました。

```mermaid
sequenceDiagram
participant A as アプリケーション本体
participant B as JobBroker
participant C as Cache Service
participant D as TimeBased Trigger
A->>B: ジョブパラメータとトリガー実行する関数を指定してジョブを登録
B->>C: ジョブパラメータとトリガー実行する関数名をキャッシュ
B->>D: トリガーを登録（トリガー実行する関数を指定）
D->>A: トリガー実行する関数を呼び出し（時間になるとイベント実行される）
A->>B: トリガー実行された関数名を指定してジョブのパラメータを取得
B->>C: ジョブパラメータをキャッシュからロード
A->>A: 無名関数にジョブパラメータを渡して実行
```

簡易的なジョブキューなシステムで非同期実行をするという点においては実現できていたのですが、イマイチでした。 ^[ジョブパラメータをコールバック関数に直接渡すことが出来ませんでした]
これを以下のようなフローにしたいと考えました。

```mermaid
sequenceDiagram
participant A as アプリケーション本体
participant B as JobBroker
participant C as Cache Service
participant D as TimeBased Trigger
A->>B: ジョブパラメータとコールバック用の関数を指定してジョブを登録
B->>C: ジョブパラメータとコールバック関数名をキャッシュ
B->>D: トリガーを登録（ジョブ実行用の共通関数を指定）
D->>A: ジョブ実行用の大本の関数呼び出し（時間になるとイベント実行される）
A->>B: トリガーイベントのオブジェクトを渡してライブラリ側のイベントハンドラーを実行
B->>C: トリガーIDからジョブパラメータとコールバック関数名をキャッシュからロード
C->>A: ジョブパラメータを指定してコールバック関数を実行
```

最後の GAS ライブラリ側からアプリケーション本体のグローバル関数を呼び出す様にしたかったのです。

## グローバル関数を動的にコールバックさせるには？

上記のフローでありますが、キャッシュされたコールバック用の関数名（文字列）から動的にコールバックさせる必要があります。
javascript で動的に関数を呼び出す方法は

* eval 関数を使う
* Function コンストラクタを使う
* window オブジェクトを使う

上記などがあります。
ただ今回は GAS になりこちらに上げた方法では使えませんでした。  
セキュリティの面で eval は論外として、google apps script ではグローバルスコープにある変数や関数にアクセスするには windows オブジェクトではなく、globalThis は使えそうでした。

## GASライブラリ内のglobalThisのスコープについて

結論からいうと、globalThis は GAS のライブラリ内ではそのままでは使えませんでした。  
原因は GAS ライブラリ内の globalThis のスコープはアプリケーション側のスコープと独立している為です。  
オフィシャルに変数のスコープ等についての制約を明記したドキュメントを見つけることは出来ませんでしたが、セキュリティ的にライブラリ側からアクセスできるとアレなので仕方ないと思います。  

ライブラリ側からアプリケーション側のグローバル関数へのアクセスがそのままでは出来ないという壁にぶつかりました。

## 参照できないなら渡してしまえ

globalThis が独立しているので、グローバルで定義された関数オブジェクトが参照できませんでした。  
ライブラリとしては関数オブジェクトが欲しいだけなのにーと頭を抱えてしまいましたが、力技でなんとかすることにしました。

* アプリケーション側
    ```ts
    function jobEventHandler(event: TimeBasedEvent): void {
        JobBroker.consumeJob(event, globalThis);
    }
    ```

* ライブラリ側
    ```ts
    public consumeJob(event: TimeBasedEvent, global: typeof globalThis): void {

        // .. snip ..

        // 関数オブジェクト取得
        const handler = global[parameter.handler];
        if (handler === undefined || typeof handler !== "function") {
            throw new TypeError();
        }

        // 関数をコールバック実行
        const result = handler(JSON.parse(parameter.parameter));
    }
    ```

アプリケーション側の globalThis を渡すので引数が1つ増えてしまいましたが、まあ許容範囲かなーと思いました。

## 最後に

ライブラリの型定義情報も更新しています。  
今回 Generic に対してジョブパラメータを型安全に利用できるようにしました。  
またコールバック関数の戻り値には真偽値を返せるようになりました。これはジョブ実行後のトリガー実行のステータスを制御できるようにする為で、ジョブが失敗した場合にログを確認しやすくする為のものです。