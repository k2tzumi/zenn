---
title: "App manifestでSlackアプリを簡単インストール＆GPTチャットボット体験"
emoji: "📦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["slack","gpt"]
published: true
---

## はじめに

皆さん。ChatGPTやBing AIを使って楽しんでいますか？  
OpenAIのアカウント登録時に**18ドル分クレジット (有効期限3ヶ月)**が付与されています。  
クレジットが勿体ないのと、技術的に興味があったのでbotでも作って見るかーというのが、今回の記事のきっかけとなっています。  

今回は単純なチャットボットを作るだけでなく、App manifestという機能を使ってSlackアプリケーションのインストールを簡単化する方法を試してみました。App manifestは以前から気になっていた機能です。

:::message
これから紹介するApp manifestの機能については現時点ではβ版となりますのでご承知おきください。
:::

## App manifestって何？

SlackのApp Manifestとは、Slackアプリの設定をYAMLまたはJSON形式でまとめたものです。

https://api.slack.com/reference/manifests#creating_manifests

App Manifestを使うと、UIやAPIを通して、あらかじめ定義された設定でSlackアプリを作成したり、既存のアプリの設定を調整したりできます。  


## Slackアプリの設定が必要なもの

Slackアプリの設定内容で代表的なものは、以下のようなものがあります。

* アプリの名前や説明、アイコンなどの基本情報
* アプリが使用するスコープや機能
例：メッセージ送信、モーダル表示、コマンド実行など
* アプリが受け取るイベントやインタラクティブ要素
例：メッセージ作成、ボタンクリック、ショートカット呼び出し
* アプリが提供するスラッシュコマンドやワークフローステップ
* アプリが必要とする環境変数やトークン

設定する項目が多数あります。
詳細な設定内容はここでは割愛しますが、Slackアプリケーションの開発してない人から見ると初見お断りな雰囲気を感じて頂けるかと思いますw  
今までにSlack botをいくつか作成してきましたが、複数のworkspaceで同じbotを使うのに毎回設定が必要で面倒くさく感じていました。

## App manifestの記述例

今回のbotでは以下のような機能を実現・実装しています。

* OAuth連携
* メンションを飛ばすと反応する
直ぐに返信できないので、まずはリアクションを付ける
* 以前の会話内容を取得して回答内容を考える
他のbot ^[自分自身のbotも含む] のメッセージに反応しないようにユーザー確認をする
* API Keyが未登録の場合は対話的に登録を促す

今回作成したBotのApp manifestは以下の様になります。

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

イベント購読や権限などは複数指定があり、毎回UIから設定するのは骨が折れます。
Slack側の仕様も理解していないと、設定自体も大変だと思います。  

## アプリケーションの登録自体もAPI経由で行う

App Manifestの登録を行うには [UI](https://api.slack.com/apps) から登録する方法がありますがAPIからも登録が可能です。API経由で登録することで後述するCredentialsのやり取りも自動化できるメリットがあります。  

https://api.slack.com/methods/apps.manifest.create

こちらのAPIですが、他のAPIとは異なり、必要となるtokenの種類がConfiguration tokenになります。  

以下のページからConfiguration tokenの発行をしてください。  

https://api.slack.com/apps

![Configuration token発行](/images/articles/introduction-of-slack-bot-with-app-manifest/configuration-token.png)

`apps.manifest.create` のAPIの呼び出しに成功するとBotアプリケーション側に設定すべきApp Credentials（Client IDやClient Secret等）がレスポンスされるので保持するようにします^[ここら辺のSlack側だけでなくBot側も設定が必要というものインストールの難易度を上げている要因だと思っています]。  
注意点としてはこの時点ではOAuthの設定はできないという点です。  
そもそもOAtuh設定するのにClient ID等が必要になるので、アプリケーションがSalck側に登録されてからじゃないとできません。  
また権限的にOAuthで付与されないと利用できない機能についても設定ができません。

## OAuthの設定は更新APIで行う

App Manifestを登録して取得できたClient IDでOAuth認証用のリダイレクトURLを発行して、App Manifestを更新させます。

https://api.slack.com/methods/apps.manifest.update

パラメータは登録時とほぼ同様で、登録時に発行されたapp_idを使う必要があります。  
OAuth設定ができるとbotのpermission設定ができるようになり、それに連動して各種イベントの購読設定やインタラクティブ周りの設定も可能になります。  
このタイミングでも諸々設定を完了させてしまいます。  ^[逆に前述のcreate時は必要な設定のみにした方が良いです]

アプリケーションの修正で利用する権限が増えた場合でも、こちらのAPIを呼び出せば反映を行うことができます。  
既存の権限から変更があった場合は、APIのレスポンスの `permissions_updated` が `true` になるので再度OAuth認証のフローに誘導し、新しいアクセス権限を承認するようにします。

## 出来たもの

前書きが長くなってしまいましたが、実際に作ったものを見てもらった方がイメージし易いので実際のインストールの手順を踏まえて説明していきます。  

リポジトリはこちらになります。

https://github.com/k2tzumi/openai-slack-bot

先に機能説明だけを軽く行うと openaiがオフィシャルでサンプル実装している [GPT Discord Bot](https://github.com/openai/gpt-discord-bot) を参考にさせて頂きGoogle Apps Script（GAS）で実装ました。  
スレッド内で質問をすると文脈を考慮して回答が得られる様になっています。  
bot自体の使い方はREADMEの `How to use` の項を参照ください。  

GPT Discord Botとの違いは以下の様になります。

* コマンド実行でなくてメンション起動  
スレッド内で途中から質問したかったのでメンションに反応するようにしています ^[Slackのスレッド内でスラッシュコマンド使えないのって何なんでしょうね？ご存知の方いらっしゃったら教えて頂きたいです]
* Slackアカウント毎にAPI Keyを保持する  
初回利用時にAPI Keyを対話的に保持させるようにしました。これでtokenを使いすぎ問題を回避させていますｗ
* 若干のPrompt Engineering  
曖昧な質問がされた場合に聞き直ししてくれるようにチューニング

## 導入手順

前提事項として以下のツール（コマンド）が必要になります

* [clasp](https://github.com/google/clasp)  
Google Apps Scriptのデプロイにclaspを使っています
* make  
手順を簡素化する為に一部処理をMakefileでコマンド化してます

以下に示す大きな２つの流れになります。

1. Botアプリケーションのデプロイ手順  
    1. リポジトリをclone  
    1. GASのプロジェクト構築〜アップロード（make push） 
    1. GASをWebAppとしてデプロイ  
1. Slackアプリケーションのインストール＆セットアップ手順  
    1. ブラウザでアプリケーションを表示してConfiguration tokenを指定してSlack workspaceにインストール  
    1. OAuth認証をしてBotアプリケーションに権限付与  

### Botアプリケーションのデプロイ手順

#### リポジトリをclone

好きなディレクトリでいつも通りgit clone
```console
$ git clone https://github.com/k2tzumi/openai-slack-bot.git
```

#### GASのプロジェクト構築〜アップロード（make push）

claspでスクリプトをdeployできるようにmakeコマンドが用意されています。  
make pushはclasp loginとclasp createを実行してGoogle Apps Scriptプロジェクトを作成し、その設定ファイルをローカルに保存します。  

:::details makeコマンド実行結果
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


Google Apps ScriptはGoogleアカウントを持っていれば誰でも利用できます。  
ただしGoogle Apps Scriptを初めて利用する場合は以下のGoogle Apps Scriptの管理画面から「設定」→「Google Apps Script API」をオンにしてください。

https://script.google.com/home/usersettings

#### GASをWebAppとしてデプロイ  

Google Apps ScriptをWebAppとしてデプロイする必要があります。  
初回では以下の手順の通りとなります。  

1. スクリプトエディタを開く
  ```console
  $ make open
  ```
  ブラウザが起動し、スクリプトエディタが開かれます
1. 「デプロイ」 → 「新しいデプロイ」を選択
![スクリプトエディタ](/images/articles/introduction-of-slack-bot-with-app-manifest/script-editor.png)
1. そのまま「デプロイ」をクリック  
![デプロイ](/images/articles/introduction-of-slack-bot-with-app-manifest/deploy.png)
makeコマンド内でWebAppとして登録されているので、画面の様にそのままデプロイするだけだと思います。
1. アクセスを承認
![デプロイ](/images/articles/introduction-of-slack-bot-with-app-manifest/authorize-access.png)
1. 実行ユーザーのアカウント選択  
![アカウント選択](/images/articles/introduction-of-slack-bot-with-app-manifest/choosen-an-account.png)
1. アプリケーションの承認  
![アプリケーション承認ダイアログ](/images/articles/introduction-of-slack-bot-with-app-manifest/verified-this-app.png)
ここが分かりづらいですが、「Advanced」をクリック「Go to openai-slack-bot(unsafe)」をクリック。。
![アプリケーション承認続き](/images/articles/introduction-of-slack-bot-with-app-manifest/continue.png)
しばらく待って
![デプロイ完了](/images/articles/introduction-of-slack-bot-with-app-manifest/deployed.png)
こちらの画面が表示されればOKです！  
アプリケーション更新時など２回目のデプロイは `make deploy` コマンドだけで済むようになっています。

### Slackアプリケーションのインストール＆セットアップ手順  

先程のデプロイ画面のURLをクリックすると以下の画面が開きます。 ^[もしURLを控え忘れていたら `make application` で最新のデプロイバージョンを選択してもURLを開けます。]

![App Manifest登録](/images/articles/introduction-of-slack-bot-with-app-manifest/create-app-manifest.png)

先に導入手順のところで説明したConfiguration tokenをこちらの画面で入力します。  
tokenが２種類ありますが、必ず `Refresh token` を指定してしてください。^[tokenの有効期限が短くRefresh tokenを指定することが推奨されています。あと後述する再インストール時にもこちらのtokenは利用します。]入力後に `Create App` ボタンをクリックしてください。
クリックによりApp Manifestが自動的に作成され、Slack上にBotアプリが仮登録されます。  

![Slackへのインストール](/images/articles/introduction-of-slack-bot-with-app-manifest/add-to-slack.png)

次の画面がこちらになります。こちらの画面から「Add to Slack」ボタンを押してください。

後は他のSlackアプリの導入と同じように権限周りの確認をしてそのままOKすれば終わりです。


![完了](/images/articles/introduction-of-slack-bot-with-app-manifest/completed.png)

こちらの画面が表示されればOKです。
後は、好きなチャネルに `@open-ai` ユーザーをinviteして試してみてください。  

#### おしまい

Google App Scriptのデプロイの方が独特でそちらの方の説明に時間がかかってしまった感がありますｗ  
SlackアプリケーションはConfiguration tokennを指定するだけで3回クリックするだけでインストール＆セットアップできます。^[Google App Scriptでうまくリダイレクトが出来なかったので3Clickになっていますが、Nodeとかで実装すれば2Clickでいけそうです。]  
内部的には設定や認証情報を多く必要としますが、SlackとBotの両方の設定を手動で変更しなくて導入ができるようになりました。  

### 補足

登録後の完了画面にある「Reinstall to Slack」ボタンは、Botアプリを更新して再デプロイする際に利用します。このボタンを押すと、App Manifestも自動的に更新されます。  
Google App Scriptは新しいデプロイを行うとバージョンが上がりアプリケーションのURLも変わります。URL変更に伴ってSlack側のイベントHook用のURLも変更しなければならなかったりしますが、前述したApp Manifestの更新APIを利用して対応しました。 

もう一つ今回アプリケーションの削除にも対応していみました。  
BotアプリケーションのURLの末尾に `?logout=true` とGetパラメータをつけてみてください。  
App Manifestの削除とBot内で保持している情報をクリアする様にしています。  
こちらを実行すると、Slackの管理画面上からアプリケーションが見れなくなりました。  
公式のドキュメントからは読み取れなかったのですが、App Manifest自体がなくなるとアプリケーションの削除と同じ扱いになっているようでした。  

## やってみた感想

今回のようにSlackの野良botを配布する場合はApp manifestの対応はかなりオススメだと感じました。  
現時点ではβ版という位置づけだと思いますが、今の所大きな躓きもありませんでした。　　

触った印象としてはSlackアプリケーションにインストーラーを内蔵している感がありました。  
READMEにインストール手順をまとめるぐらいならサクッとApp Manifestを出力してAPI連携まで行ったほうがコスパが良いと感じました。  

GPTチャットボットを作ってみた感想になりますが、openaiのAPIを使えば簡単に実現ができることが理解できたのが収穫でした。  
どうやって会話の文脈を考慮して回答を得ているのだろう？と興味津々でしたが、 [completions](https://platform.openai.com/docs/api-reference/completions) という一つのAPIで実現できているのを知って驚きでした。  
質問と回答をすべてpromptに突っ込んで良しなに文脈を汲み取ってくれていて凄い！という感想でした。  
ただ使っている内に単純に質問と回答を混ぜてprompt指定すると思ったより精度が良くないということに気付きました。  
その後色々な記事を参考にpromptを調整してみたりしました。  
今後もpromptを弄ってチューニングを試みたいと思います。