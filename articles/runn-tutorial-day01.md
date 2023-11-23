---
title: "【Day1】runnをインストールしてみよう / runnチュートリアル"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["e2e","automation","api","チュートリアル","アドベントカレンダー"]
published: true
published_at: 2023-12-01 00:00
---

:::message
この記事は [runnチュートリアル Advent Calendar 2023](https://qiita.com/advent-calendar/2023/runn-tutorial)の 12/01 配信になります。
:::

## はじめに

一人アドベントカレンダーとしスタートします。  
本記事は API シナリオテストツールでもある [runn](https://github.com/k1LoW/runn) のチュートリアルをステップバイステップで理解して貰おう！というのが趣旨です。  
25 日全部理解したら一人で runn を使って API シナリオテストや、ちょっとした API と連携する自動化処理までをできるようになること目標にしています。  
runn is 何？という方は、以下に紹介記事を書いていますのでよろしくお願いします。

https://zenn.dev/katzumi/articles/api-scenario-testing-with-runn

チュートリアルを実際に試してみて、もし躓いた箇所がありましたら記事のコメントをして頂ければと思います。

では早速いってみましょう

## runnをインストールしてみよう

runn は golang で書かれた CLI ツールなっており、導入は簡単です。
インストール方法はいくつかありますが、代表的なものだと以下の様になります


* Homebrew  
    ```bash
    $ brew install k1LoW/tap/runn
    ```
    :::details 実行結果
    Running `brew update --auto-update`...
    ==> Auto-updated Homebrew!
    Updated 3 taps (yt-dlp/taps, homebrew/core and homebrew/cask).
    ==> New Formulae
    abi3audit                   certgraph                   gitsign                     osctrl-cli                  python-mpmath               sloth
    action-validator            certsync                    gittuf                      oslo                        python-networkx             snyk-cli
    ain                         cfripper                    gotpm                       pan                         python-sympy                spglib
    ali                         chainsaw                    graphqlviz                  patat                       raven                       sqlfmt
    amass                       changie                     haiti                       patch-package               redwax-tool                 ssh-mitm
    apkleaks                    cliam                       immer                       pciutils                    reflex                      sshportal
    appstream                   crunchy-cli                 intercept                   pdfalyzer                   regclient                   tailspin
    asitop                      csprecon                    invoice                     perl-xml-parser             rekor-cli                   terraform-local
    auditwheel                  daktilo                     jprq                        phylum-cli                  richgo                      three-body
    authz0                      der-ascii                   kew                         pmix                        ruler                       tlrc
    awscli-local                dicebear                    lager                       presenterm                  scarb                       uvicorn
    badkeys                     dockerfile-language-server  libjuice                    python-brotli               scikit-image                vet
    bao                         faircamp                    libsail                     python-cycler               scilla                      vulsio-gost
    biodiff                     favirecon                   mediamtx                    python-dateutil             shell2http                  whisper-cpp
    bkcrack                     flyscrape                   minder                      python-docopt               shellspec                   witness
    bob                         frozen                      modsurfer                   python-filelock             sigma-cli                   xeol
    bomber                      geoip2fast                  node@20                     python-jinja                signmykey                   zix
    bounceback                  ghc@9.6                     noir                        python-kiwisolver           sigstore                    zug
    cariddi                     git-mediate                 orbuculum                   python-mako                 skate
    ==> New Casks
    affine                  commandpost             fossa                   jazz2-resurrection      mitmproxy               ovito                   xsplit-vcam
    batchoutput-pdf         deskpad                 gstreamer-development   keymapp                 music-decoy             ovito-pro
    btp                     deveco-studio           gstreamer-runtime       libndi                  ok-json                 rode-central
    clibor                  douyin-chat             hoppscotch              metamer                 orka3                   simplex

    You have 9 outdated formulae installed.

    ==> Tapping k1low/tap
    Cloning into '/opt/homebrew/Library/Taps/k1low/homebrew-tap'...
    remote: Enumerating objects: 2745, done.
    remote: Counting objects: 100% (110/110), done.
    remote: Compressing objects: 100% (80/80), done.
    remote: Total 2745 (delta 69), reused 64 (delta 30), pack-reused 2635
    Receiving objects: 100% (2745/2745), 406.02 KiB | 3.36 MiB/s, done.
    Resolving deltas: 100% (1761/1761), done.
    Tapped 51 formulae (65 files, 555.8KB).
    ==> Fetching k1low/tap/runn
    ==> Downloading https://github.com/k1LoW/runn/releases/download/v0.90.2/runn_v0.90.2_darwin_arm64.zip
    ==> Downloading from https://objects.githubusercontent.com/github-production-release-asset-2e65be/466909944/7154f52d-a6a5-4191-9e0c-a56f00dea5dc?X-Amz-Algorithm=AWS4-HM
    ################################################################################################################################################################# 100.0%
    ==> Installing runn from k1low/tap
    ==> Caveats
    zsh completions have been installed to:
    /opt/homebrew/share/zsh/site-functions
    ==> Summary
    🍺  /opt/homebrew/Cellar/runn/0.90.2: 8 files, 52.6MB, built in 2 seconds
    ==> Running `brew cleanup runn`...
    Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
    Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
    ==> `brew cleanup` has not been run in the last 30 days, running now...
    Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
    Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
    Removing: /Users/bob/Library/Caches/Homebrew/fontconfig--2.14.2... (627.6KB)
    Removing: /opt/homebrew/Cellar/freetype/2.13.1... (67 files, 2.4MB)
    Removing: /opt/homebrew/Cellar/gettext/0.21.1... (1,983 files, 20.9MB)
    Removing: /Users/bob/Library/Caches/Homebrew/giflib--5.2.1... (147.8KB)
    Removing: /Users/bob/Library/Caches/Homebrew/glib--2.78.0... (6.6MB)
    Removing: /opt/homebrew/Cellar/jpeg-turbo/2.1.5.1... (44 files, 2.5MB)
    Removing: /Users/bob/Library/Caches/Homebrew/libpng--1.6.40... (447.9KB)
    Removing: /Users/bob/Library/Caches/Homebrew/lz4--1.9.4... (251.0KB)
    Removing: /Users/bob/Library/Caches/Homebrew/nodebrew--1.2.0... (10.7KB)
    Removing: /Users/bob/Library/Caches/Homebrew/oniguruma--6.9.8... (440.9KB)
    Removing: /Users/bob/Library/Caches/Homebrew/openssl@3--3.1.3... (7.7MB)
    Removing: /Users/bob/Library/Caches/Homebrew/pcre2--10.42... (2.0MB)
    Removing: /Users/bob/Library/Caches/Homebrew/qt@5--5.15.10... (114.7MB)
    Removing: /Users/bob/Library/Caches/Homebrew/sqlite--3.43.2... (2.1MB)
    Removing: /Users/bob/Library/Caches/Homebrew/xpdf--4.04... (5.4MB)
    Removing: /Users/bob/Library/Caches/Homebrew/xz--5.4.4... (660.5KB)
    Removing: /Users/bob/Library/Caches/Homebrew/yt-dlp--Brotli--1.0.9.zip... (498.2KB)
    Removing: /Users/bob/Library/Caches/Homebrew/yt-dlp--websockets--10.3.tar.gz... (82.1KB)
    Removing: /Users/bob/Library/Caches/Homebrew/yt-dlp--certifi--2022.6.15.tar.gz... (158.0KB)
    Removing: /Users/bob/Library/Caches/Homebrew/yt-dlp--mutagen--1.45.1.tar.gz... (1.2MB)
    Removing: /Users/bob/Library/Caches/Homebrew/yt-dlp--2023.10.13.tar.gz... (2.7MB)
    Removing: /Users/bob/Library/Caches/Homebrew/yt-dlp--pycryptodomex--3.15.0.tar.gz... (4.3MB)
    Removing: /Users/bob/Library/Caches/Homebrew/zstd--1.5.5... (784.4KB)
    Removing: /Users/bob/Library/Caches/Homebrew/lz4_bottle_manifest--1.9.4... (8KB)
    Removing: /Users/bob/Library/Caches/Homebrew/gettext_bottle_manifest--0.21.1... (10.8KB)
    Removing: /Users/bob/Library/Caches/Homebrew/libtiff_bottle_manifest--4.5.1... (9.8KB)
    Removing: /Users/bob/Library/Caches/Homebrew/qt@5_bottle_manifest--5.15.10... (43KB)
    Removing: /Users/bob/Library/Caches/Homebrew/freetype_bottle_manifest--2.13.1... (8.3KB)
    Removing: /Users/bob/Library/Caches/Homebrew/pcre2_bottle_manifest--10.42... (8.5KB)
    Removing: /Users/bob/Library/Caches/Homebrew/xpdf_bottle_manifest--4.04-1... (23.7KB)
    Removing: /Users/bob/Library/Caches/Homebrew/giflib_bottle_manifest--5.2.1... (8.9KB)
    Removing: /Users/bob/Library/Caches/Homebrew/actionlint_bottle_manifest--1.6.25... (7.3KB)
    Removing: /Users/bob/Library/Caches/Homebrew/nodebrew_bottle_manifest--1.2.0... (1.7KB)
    Removing: /Users/bob/Library/Caches/Homebrew/xz_bottle_manifest--5.4.3... (7.5KB)
    Removing: /Users/bob/Library/Caches/Homebrew/fontconfig_bottle_manifest--2.14.2... (9KB)
    Removing: /Users/bob/Library/Caches/Homebrew/zstd_bottle_manifest--1.5.5... (8.3KB)
    Removing: /Users/bob/Library/Caches/Homebrew/webp_bottle_manifest--1.3.0_1... (12.5KB)
    Removing: /Users/bob/Library/Caches/Homebrew/glib_bottle_manifest--2.76.3... (13.3KB)
    Removing: /Users/bob/Library/Caches/Homebrew/jpeg-turbo_bottle_manifest--2.1.5.1... (8.3KB)
    Removing: /Users/bob/Library/Caches/Homebrew/libpng_bottle_manifest--1.6.40... (7.9KB)
    Removing: /Users/bob/Library/Caches/Homebrew/Cask/clipy--1.2.1.dmg... (10.6MB)
    Removing: /Users/bob/Library/Caches/Homebrew/Cask/google-japanese-ime--2.28.5050.dmg... (64.4MB)
    Removing: /Users/bob/Library/Logs/Homebrew/yt-dlp... (11 files, 674.5KB)
    Removing: /Users/bob/Library/Logs/Homebrew/glib... (64B)
    Removing: /Users/bob/Library/Logs/Homebrew/openssl@3... (64B)
    Removing: /Users/bob/Library/Logs/Homebrew/ca-certificates... (64B)
    Removing: /Users/bob/Library/Logs/Homebrew/python@3.11... (2 files, 2.5KB)
    Pruned 0 symbolic links and 4 directories from /opt/homebrew
    :::

* Golang  
    ```bash
    $ go install github.com/k1LoW/runn/cmd/runn@latest
    ```
    :::details 実行結果
    :::

その他のインストール方法はオフィシャルのページを参照ください

https://github.com/k1LoW/runn#install

以下のコマンドで正しくインストールできたか？確認できます。

```console
$ runn --version
runn version 0.90.2
```

明日は「Hello world!」です。

https://zenn.dev/katzumi/articles/runn-tutorial-day02