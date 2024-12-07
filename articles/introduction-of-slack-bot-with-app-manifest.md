---
title: "App manifestでSlackアプリを簡単インストール＆GPTチャットボット体験"
emoji: "📦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["slack","gpt"]
published: true
---

## はじめに

皆さん。ChatGPT や Bing AI を使って楽しんでいますか？  
OpenAI のアカウント登録時に**18 ドル分クレジット (有効期限 3 ヶ月)**が付与されています。  
クレジットが勿体ないのと、技術的に興味があったので bot でも作って見るかーというのが、今回の記事のきっかけとなっています。  

今回は単純なチャットボットを作るだけでなく、App manifest という機能を使って Slack アプリケーションのインストールを簡単化する方法を試してみました。App manifest は以前から気になっていた機能です。

:::message
これから紹介する App manifest の機能については現時点ではβ版となりますのでご承知おきください。
:::

## App manifestって何？

Slack の App Manifest とは、Slack アプリの設定を YAML または JSON 形式でまとめたものです。

https://api.slack.com/reference/manifests#creating_manifests

App Manifest を使うと、UI や API を通して、あらかじめ定義された設定で Slack アプリを作成したり、既存のアプリの設定を調整したりできます。  


## Slackアプリの設定が必要なもの

Slack アプリの設定内容で代表的なものは、以下のようなものがあります。

* アプリの名前や説明、アイコンなどの基本情報
* アプリが使用するスコープや機能
例：メッセージ送信、モーダル表示、コマンド実行など
* アプリが受け取るイベントやインタラクティブ要素
例：メッセージ作成、ボタンクリック、ショートカット呼び出し
* アプリが提供するスラッシュコマンドやワークフローステップ
* アプリが必要とする環境変数やトークン

設定する項目が多数あります。
詳細な設定内容はここでは割愛しますが、Slack アプリケーションの開発してない人から見ると初見お断りな雰囲気を感じて頂けるかと思います w  
今までに Slack bot をいくつか作成してきましたが、複数の workspace で同じ bot を使うのに毎回設定が必要で面倒くさく感じていました。

## App manifestの記述例

今回の bot では以下のような機能を実現・実装しています。

* OAuth 連携
* メンションを飛ばすと反応する
直ぐに返信できないので、まずはリアクションを付ける
* 以前の会話内容を取得して回答内容を考える
他の bot ^[自分自身のbotも含む] のメッセージに反応しないようにユーザー確認をする
* API Key が未登録の場合は対話的に登録を促す

今回作成した Bot の App manifest は以下の様になります。

:::details App manifest
```yaml
display_information:
  name: OpenAI Bot
features:
  bot_user:
    display_name: open-ai
    always_online: true
oauth_config:
  redirect_urls:
    - OAuthのリダイレクトURL
  scopes:
    bot:
      - chat:write
      - channels:history
      - groups:history
      - mpim:history
      - im:history
      - app_mentions:read
      - reactions:write
      - users:read
settings:
  event_subscriptions:
    request_url: イベントHook用のURL
    bot_events:
      - app_mention
      - message.channels
      - message.groups
  interactivity:
    is_enabled: true
    request_url: 対話的なアクションの受信用URL
  org_deploy_enabled: false
  socket_mode_enabled: false
  token_rotation_enabled: false
```
:::

イベント購読や権限などは複数指定があり、毎回 UI から設定するのは骨が折れます。
Slack 側の仕様も理解していないと、設定自体も大変だと思います。  

## アプリケーションの登録自体もAPI経由で行う

App Manifest の登録を行うには [UI](https://api.slack.com/apps) から登録する方法がありますが API からも登録が可能です。API 経由で登録することで後述する Credentials のやり取りも自動化できるメリットがあります。  

https://api.slack.com/methods/apps.manifest.create

こちらの API ですが、他の API とは異なり、必要となる token の種類が Configuration token になります。  

以下のページから Configuration token の発行をしてください。  

https://api.slack.com/apps

![Configuration token 発行](/images/articles/introduction-of-slack-bot-with-app-manifest/configuration-token.png =800x)

`apps.manifest.create` の API の呼び出しに成功すると Bot アプリケーション側に設定すべき App Credentials（Client ID や Client Secret 等）がレスポンスされるので保持するようにします^[ここら辺のSlack側だけでなくBot側も設定が必要というものインストールの難易度を上げている要因だと思っています]。  
注意点としてはこの時点では OAuth の設定はできないという点です。  
そもそも OAtuh 設定するのに Client ID 等が必要になるので、アプリケーションが Salck 側に登録されてからじゃないとできません。  
また権限的に OAuth で付与されないと利用できない機能についても設定ができません。

## OAuthの設定は更新APIで行う

App Manifest を登録して取得できた Client ID で OAuth 認証用のリダイレクト URL を発行して、App Manifest を更新させます。

https://api.slack.com/methods/apps.manifest.update

パラメータは登録時とほぼ同様で、登録時に発行された app_id を使う必要があります。  
OAuth 設定ができると bot の permission 設定ができるようになり、それに連動して各種イベントの購読設定やインタラクティブ周りの設定も可能になります。  
このタイミングでも諸々設定を完了させてしまいます。  ^[逆に前述のcreate時は必要な設定のみにした方が良いです]

アプリケーションの修正で利用する権限が増えた場合でも、こちらの API を呼び出せば反映を行うことができます。  
既存の権限から変更があった場合は、API のレスポンスの `permissions_updated` が `true` になるので再度 OAuth 認証のフローに誘導し、新しいアクセス権限を承認するようにします。

## 出来たもの

前書きが長くなってしまいましたが、実際に作ったものを見てもらった方がイメージし易いので実際のインストールの手順を踏まえて説明していきます。  

リポジトリはこちらになります。

https://github.com/k2tzumi/openai-slack-bot

先に機能説明だけを軽く行うと openai がオフィシャルでサンプル実装している [GPT Discord Bot](https://github.com/openai/gpt-discord-bot) を参考にさせて頂き Google Apps Script（GAS）で実装ました。  
スレッド内で質問をすると文脈を考慮して回答が得られる様になっています。  
bot 自体の使い方は README の `How to use` の項を参照ください。  

GPT Discord Bot との違いは以下の様になります。

* コマンド実行でなくてメンション起動  
スレッド内で途中から質問したかったのでメンションに反応するようにしています ^[Slackのスレッド内でスラッシュコマンド使えないのって何なんでしょうね？ご存知の方いらっしゃったら教えて頂きたいです]
* Slack アカウント毎に API Key を保持する  
初回利用時に API Key を対話的に保持させるようにしました。これで token を使いすぎ問題を回避させていますｗ
* 若干の Prompt Engineering  
曖昧な質問がされた場合に聞き直ししてくれるようにチューニング

## 導入手順

前提事項として以下のツール（コマンド）が必要になります

* [clasp](https://github.com/google/clasp)  
Google Apps Script のデプロイに clasp を使っています
* make  
手順を簡素化する為に一部処理を Makefile でコマンド化してます

以下に示す大きな２つの流れになります。

1. Bot アプリケーションのデプロイ手順  
    1. リポジトリを clone  
    1. GAS のプロジェクト構築〜アップロード（make push） 
    1. GAS を WebApp としてデプロイ  
1. Slack アプリケーションのインストール＆セットアップ手順  
    1. ブラウザでアプリケーションを表示して Configuration token を指定して Slack workspace にインストール  
    1. OAuth 認証をして Bot アプリケーションに権限付与  

### Botアプリケーションのデプロイ手順

#### リポジトリをclone

好きなディレクトリでいつも通り git clone
```console
$ git clone https://github.com/k2tzumi/openai-slack-bot.git
```

#### GASのプロジェクト構築〜アップロード（make push）

clasp でスクリプトを deploy できるように make コマンドが用意されています。  
make push は clasp login と clasp create を実行して Google Apps Script プロジェクトを作成し、その設定ファイルをローカルに保存します。  

:::details make コマンド実行結果
```log
make login
clasp login
Warning: You seem to already be logged in *globally*. You have a ~/.clasprc.json
Logging in globally…
🔑 Authorize clasp by visiting this url:
https://accounts.google.com/o/oauth2/v2/auth?access_type=offline&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fscript.deployments%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fscript.projects%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fscript.webapp.deploy%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fdrive.metadata.readonly%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fdrive.file%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fservice.management%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Flogging.read%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.profile%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform&response_type=code&client_id=1072944905499-vm2v2i5dvn0a0d2o4ca36i1vge8cvbn0.apps.googleusercontent.com&redirect_uri=http%3A%2F%2Flocalhost%3A59106

Authorization successful.

Default credentials saved to: /Users/username/.clasprc.json.
clasp create --title openai-slack-bot --type webapp --rootDir ./src
Created new webapp script: https://script.google.com/d/{script-id}/edit
mv src/.clasp.json .
clasp setting fileExtension ts
Updated "fileExtension": "" → "ts"
# clasp setting filePushOrder
sed -i -e 's/}/,"filePushOrder":["src\/OAuth2Handler.ts","src\/SlackBaseHandler.ts","src\/BaseError.ts"]}/' .clasp.json
rm .clasp.json-e
npm ci

added 595 packages, and audited 596 packages in 6s

122 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm run lint

> openai-slack-bot@v0.0.2 lint
> eslint --fix src/**/*.ts

clasp push -f
⠋ Pushing files…Detected filePushOrder setting. Pushing these files first:
└─ src/OAuth2Handler.ts
└─ src/SlackBaseHandler.ts
└─ src/BaseError.ts

└─ src/OAuth2Handler.ts
└─ src/SlackBaseHandler.ts
└─ src/BaseError.ts
└─ src/appsscript.json
└─ src/CallbackEventHandler.ts
└─ src/Code.ts
└─ src/InteractivityHandler.ts
└─ src/NetworkAccessError.ts
└─ src/OpenAiClient.ts
└─ src/slack/types/callback-events.d.ts
└─ src/slack/types/index.d.ts
└─ src/slack/types/interactivity.d.ts
└─ src/slack/types/slash-command.d.ts
└─ src/slack/types/tools.d.ts
└─ src/SlackApiClient.ts
└─ src/SlackConfigurator.ts
└─ src/SlackCredentialStore.ts
└─ src/SlackHandler.ts
└─ src/SlackWebhooks.ts
└─ src/SlashCommandHandler.ts
└─ src/UserCredentialStore.ts
Pushed 21 files.
```
:::


Google Apps Script は Google アカウントを持っていれば誰でも利用できます。  
ただし Google Apps Script を初めて利用する場合は以下の Google Apps Script の管理画面から「設定」→「Google Apps Script API」をオンにしてください。

https://script.google.com/home/usersettings

#### GASをWebAppとしてデプロイ  

Google Apps Script を WebApp としてデプロイする必要があります。  
初回では以下の手順の通りとなります。  

1. スクリプトエディタを開く
  ```console
  $ make open
  ```
  ブラウザが起動し、スクリプトエディタが開かれます
1. 「デプロイ」→「新しいデプロイ」を選択
![スクリプトエディタ](/images/articles/introduction-of-slack-bot-with-app-manifest/script-editor.png =800x)
1. そのまま「デプロイ」をクリック  
![デプロイ](/images/articles/introduction-of-slack-bot-with-app-manifest/deploy.png =800x)
make コマンド内で WebApp として登録されているので、画面の様にそのままデプロイするだけだと思います。
1. アクセスを承認
![デプロイ](/images/articles/introduction-of-slack-bot-with-app-manifest/authorize-access.png =800x)
1. 実行ユーザーのアカウント選択  
![アカウント選択](/images/articles/introduction-of-slack-bot-with-app-manifest/choosen-an-account.png =800x)
1. アプリケーションの承認  
![アプリケーション承認ダイアログ](/images/articles/introduction-of-slack-bot-with-app-manifest/verified-this-app.png =800x)
ここが分かりづらいですが、「Advanced」をクリック「Go to openai-slack-bot(unsafe)」をクリック。。
![アプリケーション承認続き](/images/articles/introduction-of-slack-bot-with-app-manifest/continue.png =800x)  
更にクリック
![アプリケーション承認](/images/articles/introduction-of-slack-bot-with-app-manifest/trust-bot.png =800x)  

しばらく待って
![デプロイ完了](/images/articles/introduction-of-slack-bot-with-app-manifest/deployed.png =800x)
こちらの画面が表示されれば OK です！  
アプリケーション更新時など２回目のデプロイは `make deploy` コマンドだけで済むようになっています。

### Slackアプリケーションのインストール＆セットアップ手順  

先程のデプロイ画面の URL をクリックすると以下の画面が開きます。 ^[もしURLを控え忘れていたら `make application` で最新のデプロイバージョンを選択してもURLを開けます。]

![App Manifest 登録](/images/articles/introduction-of-slack-bot-with-app-manifest/create-app-manifest.png =800x)

先に導入手順のところで説明した Configuration token をこちらの画面で入力します。  
token が２種類ありますが、必ず `Refresh token` を指定してしてください。^[tokenの有効期限が短くRefresh tokenを指定することが推奨されています。あと後述する再インストール時にもこちらのtokenは利用します。]入力後に `Create App` ボタンをクリックしてください。
クリックにより App Manifest が自動的に作成され、Slack 上に Bot アプリが仮登録されます。  

![Slack へのインストール](/images/articles/introduction-of-slack-bot-with-app-manifest/add-to-slack.png =800x)

次の画面がこちらになります。こちらの画面から「Add to Slack」ボタンを押してください。

![OAuth 認証](/images/articles/introduction-of-slack-bot-with-app-manifest/oauth.png =800x)

後は他の Slack アプリの導入と同じように権限周りの確認をしてそのまま OK すれば終わりです。


![完了](/images/articles/introduction-of-slack-bot-with-app-manifest/completed.png =800x)

こちらの画面が表示されれば OK です。
後は、好きなチャネルに `@open-ai` ユーザーを invite して試してみてください。  

#### おしまい

Google App Script のデプロイの方が独特でそちらの方の説明に時間がかかってしまった感がありますｗ  
Slack アプリケーションは Configuration tokenn を指定するだけで 3 回クリックするだけでインストール＆セットアップできます。^[Google App Scriptでうまくリダイレクトが出来なかったので3Clickになっていますが、Nodeとかで実装すれば2Clickでいけそうです。]  
内部的には設定や認証情報を多く必要としますが、Slack と Bot の両方の設定を手動で変更しなくて導入ができるようになりました。  

### 補足

登録後の完了画面にある「Reinstall to Slack」ボタンは、Bot アプリを更新して再デプロイする際に利用します。このボタンを押すと、App Manifest も自動的に更新されます。  
Google App Script は新しいデプロイを行うとバージョンが上がりアプリケーションの URL も変わります。URL 変更に伴って Slack 側のイベント Hook 用の URL も変更しなければならなかったりしますが、前述した App Manifest の更新 API を利用して対応しました。 

もう1つ今回アプリケーションの削除にも対応していみました。  
Bot アプリケーションの URL の末尾に `?logout=true` と Get パラメータをつけてみてください。  
App Manifest の削除と Bot 内で保持している情報をクリアする様にしています。  
こちらを実行すると、Slack の管理画面上からアプリケーションが見れなくなりました。  
公式のドキュメントからは読み取れなかったのですが、App Manifest 自体がなくなるとアプリケーションの削除と同じ扱いになっているようでした。  

## やってみた感想

今回のように Slack の野良 bot を配布する場合は App manifest の対応はかなりオススメだと感じました。  
現時点ではβ版という位置づけだと思いますが、今の所大きな躓きもありませんでした。

触った印象としては Slack アプリケーションにインストーラーを内蔵している感がありました。  
README にインストール手順をまとめるぐらいならサクッと App Manifest を出力して API 連携まで行ったほうがコスパが良いと感じました。  

GPT チャットボットを作ってみた感想になりますが、openai の API を使えば簡単に実現ができることが理解できたのが収穫でした。  
どうやって会話の文脈を考慮して回答を得ているのだろう？と興味津々でしたが、 [completions](https://platform.openai.com/docs/api-reference/completions) という1つの API で実現できているのを知って驚きでした。  
質問と回答をすべて prompt に突っ込んで良しなに文脈を汲み取ってくれていて凄い！という感想でした。  
ただ使っている内に単純に質問と回答を混ぜて prompt 指定すると思ったより精度が良くないということに気付きました。  
その後色々な記事を参考に prompt を調整してみたりしました。  
今後も prompt を弄ってチューニングを試みたいと思います。