---
title: "変数を外部から与えてみる"
free: true
---

# 変数を外部から与えてみる

[前のチャプタ](https://zenn.dev/katzumi/books/runn-tutorial/viewer/vars) で変数の定義と参照する方法を説明しました。 
今回は変数を外部から与える方法について説明します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day06/env.yml

変数を増やしていたりもしますが、増えた記述方法は 1 つのみです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day06/env.yml#L5

`${USER:-katzumi}` の箇所です。
Docker compose の yaml を記述した人ならお馴染みの環境変数展開が可能になっています。

こちらの定義で、`USER` という環境変数を参照しています。
また `${変数:-default}` という記法を行うとデフォルト値が適用されます。  
runn に環境変数を与えながら実行するには以下のようにします。

```console
$ USER=zenn runn run day06/env.yml --debug
```

なお一部 OS やシェルではデフォルトで `USER` が定義されていますので未指定の場合に 404 エラーが発生するかもしれませんのでご注意ください。

こちらの環境変数の展開を利用すると、環境依存の記述を排除できます。

* 1 つの runbook で本番環境とテスト環境の実行を切替る
* 環境変数によって動作を変更させることができる

といったことが出来るようになりますので、意識的に環境変数を埋め込んでいくことをオススメします。
