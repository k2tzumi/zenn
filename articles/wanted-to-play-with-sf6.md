---
title: "MacでもSF6したい"
emoji: "👊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["mac","Game"]
published: true
---

:::message
本記事の内容は基本全て自己責任でお願いします。必ずファイルをバックアップしてから試しください。
:::

## Game Porting Toolkit 2とは？

Game Porting Toolkit 2(以下 GPTK2)は、Apple が開発したソフトウェアツールキットです。
これは、Windows ゲームを macOS に移植するプロセスを簡素化することを目的としています。

* 発表時期と概要  
前進の Game Porting Toolkit は 2023 年 6 月 5 日に開催された WWDC23（Worldwide Developers Conference 2023）で初めて発表されました。
2024 年 06 月 10 日に開催した WWDC24 でアップデート内容が発表されました。

https://applech2.com/archives/20240611-game-porting-toolkit-2.html


* 主な特徴  
    1. Windows ゲームを macOS で動作させるためのトランスポート層を提供する。
    1. DirectX 12 を Metal API に変換する機能がある。
    1. パフォーマンス分析ツールが含まれており、移植プロセスを最適化できる。

* 扱う上での注意点
    1. 趣旨： ゲーム開発者向けに移植を促進する為の開発ツールである。
    1. 互換性：すべてのゲームが完全に互換性があるわけではありません。一部のゲームは追加の調整が必要。
    1. パフォーマンス：移植されたゲームは、ネイティブな macOS ゲームほどのパフォーマンスを発揮しない可能性がある。
    1. 技術的知識：効果的に使用するには、ゲーム開発と macOS プラットフォームに関する一定レベルの技術的知識が必要。
    1. Apple Silicon の最適化：M1 チップ以降の Apple Silicon デバイスで最適なパフォーマンスを得るには、追加の最適化が必要。

このツールキットは、macOS ゲームライブラリを拡大し、クロスプラットフォーム開発を促進することを目的としています。  
ただし、その使用にはいくつかの技術的およびその他考慮事項があることに注意が必要です。
詳細な情報や最新のアップデートについては、Apple のデベロッパーウェブサイトを確認することをお勧めします。

## 命令セットのサポート拡大による期待

GPTK2 から AVX2 命令セットがサポートされたことによって、今まで動かなかった Windows 用ゲームをプレイできる可能性が出てきました。

https://applech2.com/archives/20240613-apple-game-porting-toolkit-2.html

こちらの記事にもありますが、動画もアップされていたので此れは！と思った次第です。

https://x.com/katzchum/status/1801928289528713501

## 参考にした記事

以降で試したことをまとめていきます。  
ただ参考記事の方がどんどん情報更新されていきますので、最新情報がほしい方はそちらを参考にしていただけると🙏。

https://www.applegamingwiki.com/wiki/Game_Porting_Toolkit


## 推奨環境

macOS Sonoma 以降を使用する必要があります。
筆者は Sequoia で検証しています。

## 手順

1. macOS Sequoia インストール（スキップ可）  
macOS Sequoia BETA Full Installer をダウンロードしてインストール
[InstallAssistant.pkg](https://swcdn.apple.com/content/downloads/56/31/062-22031-A_W5CYUPUHNT/ud3vjnbs2w5yxby2v4eqhwkfbzx2xnjnk3/InstallAssistant.pkg)
2. Homebrew（X86 版）インストール  
    ```
    arch -x86_64 zsh 
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    alias brew86=/usr/local/bin/brew
    ```
    既存の arm 版の brew とは別にインストールされる形となります。
    パスが arm 版と異なる為、brew86 という alias を作成しています。
3. GPTK2 ( Prebuild 版) インストール
    ```
    brew86 install --cask --no-quarantine gcenx/wine/game-porting-toolkit
    ```
    [オフィシャル版](http://github.com/apple/homebrew-apple)があるのですが、こちらはビルド出来なかったので大人しく Prebuild を使いましょう。インストールも早いし。
4. ツールキットの準備  
    [Apple Developer Downloads site](https://developer.apple.com/download/all/?q=game) から Evaluation environment for Windows games 2.0 beta をダウンロードして dmg をマウントしておいてください。（要 Apple アカウント）  
    以下のコマンドでライブラリディレクトリを Wine のライブラリディレクトリにコピーしてください。
    ```
    ditto /Volumes/Evaluation\ environment\ for\ Windows\ games\ 2.0/redist/lib/ `brew86 --prefix game-porting-toolkit`/lib/ 
    cp /Volumes/Evaluation\ environment\ for\ Windows\ games\ 2.0/gameportingtoolkit* /usr/local/bin
    ```
5. Steam インストール  
    Steam のインストールと初回ログインがうまくいかないケースがある為、OS の言語設定を English にしてください。  
    Steam のオフィシャルサイトから [Windows 版のインストーラー](https://cdn.akamai.steamstatic.com/client/installer/SteamSetup.exe) をダウンロードしてください。
    ```
    WINEPREFIX=~/Steam wine64 winecfg
    WINEPREFIX=~/Steam MTL_HUD_ENABLED=0 WINEESYNC=1 wine64 ~/Downloads/SteamSetup.exe
    ```
    1 つ目のコマンドで WINEPREFIX のディレクトリが作成されます。  
    Windows バージョンが 10 になっていることを確認してください。  
    ![winecfg](/images/articles/wanted-to-play-with-sf6/wine-config.png)
    2 つ目のコマンドで Steam のインストールを行います。
    ![steam setupの最初の画面](/images/articles/wanted-to-play-with-sf6/steam-setup1.png)
    言語設定は English で OK です。
    ![言語設定](/images/articles/wanted-to-play-with-sf6/steam-setup2.png)
    インストールパスはそのままで。
    ![インストールパス](/images/articles/wanted-to-play-with-sf6/steam-setup3.png)
    以下の画面が表示されれば OK です。Run Steam のチェックを外して Finish で終わりです。
    ![コンプリート画面](/images/articles/wanted-to-play-with-sf6/steam-setup4.png)
6. Steam 起動  
    取り敢えず以下のコマンドで Steam が起動できます。
    ```
    WINEPREFIX=~/Steam MTL_HUD_ENABLED=1 WINEESYNC=1 wine64 'C:\Program Files (x86)\Steam\Steam.exe'
    ```
    真っ黒な画面が表示されてログイン出来ない場合は、OS の言語設定を English になっているか？確認してみてください。
    ログインが出来たらあとはお目当てのゲームをダウンロードして実行してみてください。

## wine の起動オプションについて（検証中）

検証したのが貧弱なマシン環境（MacBook Air(M2, 2022) 16 GB）だったので、 SF6 のデモ版では 60 FPS は出ませんでした。  
25-35 FPS ぐらいな感じでした。エフェクトが少ない状態であれば、 60 FPS がでる瞬間もありますが、動くぞ。という程度で Playable な感じではありませんでした。  
今は、 Wine の設定や起動オプションを最適化して模索している最中です。  
今の所、以下のコマンドに落ち着いています。  
```
MTL_HUD_ENABLED=1 WINEESYNC=1 DXVK_ASYNC=1 ROSETTA_ADVERTISE_AVX=1 WINEPREFIX=~/Steam wine64 'C:\Program Files (x86)\Steam\Steam.exe'
```
他にもいくつかオプションがありそうですが、違いがわかっていません。  
* WINEDLLOVERRIDES  
DDL読み込み方法を変更して、パフォーマンスに影響があることはわかっていますが、どうするのが正解なのかわかっていません。  
設定すると変わらないか、遅くなる一方です。
* DXVK_HUD
* METAL_CAPTURE_ENABLED
* WINEMSYNC

## 最後に

色々調べていますが、M3 以降のマシンであれば、もう少しましに動くと考えています。ただ現状では環境が手元になく検証できない状況です。  
もし試してみて頂けた方がいらっしゃいましたら、利用した起動オプション・FPS 等を教えて頂けますと幸いです。
