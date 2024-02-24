---
title: "アクセスログからシナリオを特定する"
free: true
---

# アクセスログからシナリオを特定する

複数シナリオテスト実行時に、エラーになった場合該当のエンドポイントへのリクエストが複数ありどのシナリオでエラーになったのか？知りたくなるケースがあります。  
runn は API にリクエスト飛ばす際にトレース用ヘッダーを付与する機能があります。  

https://github.com/k2tzumi/runn-tutorial/blob/main/day27/trace.yml

今回の新しい要素は一点のみです

https://github.com/k2tzumi/runn-tutorial/blob/main/day27/trace.yml#L3-L5

HttpRunner に `trace: true` をつけるだけです。  
こちらをつけて実行すると以下の様になります。

```console
$ runn run day27/trace.yml --debug
Run "初回ステップ" on "トレースヘッダを有効にする（ヘッダ名指定あり）".steps.step0
-----START HTTP REQUEST-----
GET /get HTTP/1.1
Host: httpbin.org
X-Runn-Trace: {"id":"0e8f015427561ab32aeba9f162fbfd8fb838f59f?step=0"}


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 359
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Sat, 24 Feb 2024 06:33:35 GMT
Server: gunicorn/19.9.0

{
  "args": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-65d98dbf-395744c275189e515be9c014", 
    "X-Runn-Trace": "{\"id\":\"0e8f015427561ab32aeba9f162fbfd8fb838f59f?step=0\"}"
  }, 
  "origin": "miserarenaiyo", 
  "url": "https://httpbin.org/get"
}

-----END HTTP RESPONSE-----
Run "test" on "トレースヘッダを有効にする（ヘッダ名指定あり）".steps.step0

Run "2回目のステップ" on "トレースヘッダを有効にする（ヘッダ名指定あり）".steps.step1
-----START HTTP REQUEST-----
POST /post HTTP/1.1
Host: httpbin.org
Content-Type: application/json
X-Runn-Trace: {"id":"0e8f015427561ab32aeba9f162fbfd8fb838f59f?step=1"}

{"foo":"bar"}
-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 528
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Sat, 24 Feb 2024 06:33:35 GMT
Server: gunicorn/19.9.0

{
  "args": {}, 
  "data": "{\"foo\":\"bar\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "13", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-65d98dbf-344a7d90300d7eda1398fef0", 
    "X-Runn-Trace": "{\"id\":\"0e8f015427561ab32aeba9f162fbfd8fb838f59f?step=1\"}"
  }, 
  "json": {
    "foo": "bar"
  }, 
  "origin": "miserarenaiyo", 
  "url": "https://httpbin.org/post"
}

-----END HTTP RESPONSE-----
Run "test" on "トレースヘッダを有効にする（ヘッダ名指定あり）".steps.step1
.

1 scenario, 0 skipped, 0 failures
```

各ステップのリクエストに `X-Runn-Trace` というカスタムヘッダーが付与されます。  
カスタムヘッダーの値は json になっており、以下のようなフォーマットでシナリオを特定する id とステップを判断できるようになっています。

```json
 {"id":"シナリオID?step=ステップ番号[&step=ステップ番号]*"}
```

今回のシナリオを例に説明をしていきます。

* steps.step0（初回ステップ）  
    ```json
    {"id":"0e8f015427561ab32aeba9f162fbfd8fb838f59f?step=0"}
    ```
* steps.step1（2 回目のステップ）  
    ```json
    {"id":"0e8f015427561ab32aeba9f162fbfd8fb838f59f?step=1"}
    ```

step の部分の番号が初回は 0、2 回目は 1 と増えていっているのがわかります。
シナリオ（runbook）を特定する id はどこからきているか？というと以下のコマンドでわかります。

```console
$ runn list --long day27/*.yml
  id:                                       desc:                                           if:  steps:  path                         
--------------------------------------------------------------------------------------------------------------------------------------
  9cd4d5910c482a9beb0a7d880344c61d78ccef6d  トレースヘッダを有効にする（ヘッダ名指定あり）            2  day27/trace-header-name.yml  
  0e8f015427561ab32aeba9f162fbfd8fb838f59f  トレースヘッダを有効にする                                2  day27/trace.yml              
```

今回は `day27/trace.yml` を実行したので id は `0e8f015427561ab32aeba9f162fbfd8fb838f59f` になっているのがわかります。

## ヘッダー名を指定する方法

先程の例では `X-Runn-Trace` というリクエストヘッダーにトレース情報が付与されていました。
こちらのヘッダーを変える方法を以下に説明します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day27/trace-header-name.yml

ヘッダー名を変更している箇所は以下のようになります。

https://github.com/k2tzumi/runn-tutorial/blob/main/day27/trace-header-name.yml#L5-L7

trace 部分を 1 つ階層を掘り下げた `headerName` で指定します。

## includeされたシナリオのトレースヘッダ

上述のシナリオは include を利用したものになっています。
この場合はどのような結果になるか？見ていきます。

```console
% runn run day27/trace-header-name.yml --debug 
Run "初回ステップ" on "トレースヘッダを有効にする（ヘッダ名指定あり）".steps.step0
-----START HTTP REQUEST-----
GET /get HTTP/1.1
Host: httpbin.org
X-Request-Id2: {"id":"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=0"}


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 360
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Sat, 24 Feb 2024 07:08:36 GMT
Server: gunicorn/19.9.0

{
  "args": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-65d995f4-10bafb3f29c4b3db58782f97", 
    "X-Request-Id2": "{\"id\":\"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=0\"}"
  }, 
  "origin": "miserarenaiyo", 
  "url": "https://httpbin.org/get"
}

-----END HTTP RESPONSE-----
Run "test" on "トレースヘッダを有効にする（ヘッダ名指定あり）".steps.step0

Run "インクルードのステップ" on "トレースヘッダを有効にする（ヘッダ名指定あり）".steps.includeStep
Run "初回ステップ" on "トレースヘッダを有効にする".steps.step0
-----START HTTP REQUEST-----
GET /get HTTP/1.1
Host: httpbin.org
X-Runn-Trace: {"id":"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=1\u0026step=0"}


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 372
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Sat, 24 Feb 2024 07:08:37 GMT
Server: gunicorn/19.9.0

{
  "args": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-65d995f5-041fe41a591122163484896a", 
    "X-Runn-Trace": "{\"id\":\"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=1\\u0026step=0\"}"
  }, 
  "origin": "miserarenaiyo", 
  "url": "https://httpbin.org/get"
}

-----END HTTP RESPONSE-----
Run "test" on "トレースヘッダを有効にする".steps.step0

Run "2回目のステップ" on "トレースヘッダを有効にする".steps.step1
-----START HTTP REQUEST-----
POST /post HTTP/1.1
Host: httpbin.org
Content-Type: application/json
X-Runn-Trace: {"id":"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=1\u0026step=1"}

{"foo":"bar"}
-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 200 OK
Content-Length: 541
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: application/json
Date: Sat, 24 Feb 2024 07:08:37 GMT
Server: gunicorn/19.9.0

{
  "args": {}, 
  "data": "{\"foo\":\"bar\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "13", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "Go-http-client/2.0", 
    "X-Amzn-Trace-Id": "Root=1-65d995f5-46925cb4412de7347eddcab0", 
    "X-Runn-Trace": "{\"id\":\"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=1\\u0026step=1\"}"
  }, 
  "json": {
    "foo": "bar"
  }, 
  "origin": "miserarenaiyo", 
  "url": "https://httpbin.org/post"
}

-----END HTTP RESPONSE-----
Run "test" on "トレースヘッダを有効にする".steps.step1
.

1 scenario, 0 skipped, 0 failures
```

それぞれのリクエストのヘッダーをまとめます。

* steps.step0（初回ステップ）  
    ```
    X-Request-Id2: {"id":"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=0"}
    ```
* steps.includeStep.step0（include 初回ステップステップ）  
    ```
    X-Runn-Trace: {"id":"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=1\u0026step=0"}
    ```
* steps.includeStep.step1（include2 回目のステップステップ）  
    ```
    X-Runn-Trace: {"id":"9cd4d5910c482a9beb0a7d880344c61d78ccef6d?step=1\u0026step=1"}
    ```

初回ステップはヘッダー名を変更しているので `X-Request-Id2` になっています。  
また実行している runbook が異なるので `9cd4d5910c482a9beb0a7d880344c61d78ccef6d` になっています。
include 元の 2 ステップ目は include されていますが、 `step=1\u0026step=0` と `step=1\u0026step=1` になっています。
json の value で escape されているので分かりづらいですが、

* include 初回ステップステップ  
`step=1&step=0`
* include2 回目のステップステップ  
`step=1&step=1`

になります。  
include されると、 `&` を付けてネストするようになります。
これで include を行う複雑なシナリオでもトレースができます。
