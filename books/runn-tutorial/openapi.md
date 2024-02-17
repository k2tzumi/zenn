---
title: "OpenAPIの仕様書通りか？テストする"
free: true
---

# OpenAPIの仕様書通りか？テストする

今までのチュートリアルで API のリクエストと期待値のレスポンスは runbook の作成者が妥当性をチェックする必要がありました。
今回は API のインターフェースの品質の向上に有効な機能を紹介します。

今回は以下の API をテストしていきます。よく見るサンプルの petstore の API です。

https://petstore3.swagger.io/

こちらをテストする runbook はこちら。

https://github.com/k2tzumi/runn-tutorial/blob/main/day18/open-api-fail.yml

ステータスを指定してペット一覧を取得する API のテストになります。

今回のチュートリアルの肝はこちらです。

https://github.com/k2tzumi/runn-tutorial/blob/main/day18/open-api-fail.yml#L3-L7

`openapi3` というパラメータを指定しています。
こちらには OpenAPI の定義ファイルを指定させます。

実行させてみます。

```console
$ runn run day18/open-api-fail.yml --verbose
=== OpenAPIのSpec通りにリクエストとレスポンスになっているか？チェックする (day18/open-api-fail.yml) ... fail
    --- Finds Pets by status (findPetsByStatus) ... fail
        Failure/Error: http request failed on "OpenAPIのSpec通りにリクエストとレスポンスになっているか？チェックする".steps.findPetsByStatus: openapi3 validation error: parameter "status" in query has an error: value is not one of the allowed values ["available","pending","sold"]
        Schema:
          {
            "default": "available",
            "enum": [
              "available",
              "pending",
              "sold"
            ],
            "type": "string"
          }
        
        Value:
          "avaliable"
        
        -----START HTTP REQUEST-----
        GET /api/v3/pet/findByStatus?status=avaliable HTTP/1.1
        Host: petstore3.swagger.io
        
        
        -----END HTTP REQUEST-----


1 scenario, 0 skipped, 1 failure
```

エラーになりました。

> openapi3 validation error: parameter "status" in query has an error: value is not one of the allowed values ["available","pending","sold"]

`status` パラメータに誤りがあるようです。
`avaliable` と typo していました。`available` が正しいようです。

この様に OpenAPI の定義ファイルを指定しておくと、リクエスト方法の誤りがあった場合、リクエスト送信時に気づくことが出来ます。
またレスポンスも自動的にチェックされます。

間違いに気づいたので、直します。

https://github.com/k2tzumi/runn-tutorial/blob/main/day18/open-api.yml

```console
$ runn run day18/open-api.yml --verbose     
=== OpenAPIのSpec通りにリクエストとレスポンスになっているか？チェックする (day18/open-api.yml) ... ok
    --- Finds Pets by status (findPetsByStatus) ... ok


1 scenario, 0 skipped, 0 failures
```

今度は OK になりました。

こちらの OpenAPI の仕様書を指定すると非常に品質の高いテストが行なえますが、敢えてリクエスト異常なテストを行いたいケースがあります。
その場合、この様にします。

https://github.com/k2tzumi/runn-tutorial/blob/main/day18/open-api-bad-request.yml

`skipValidateRequest: true` にします。

https://github.com/k2tzumi/runn-tutorial/blob/main/day18/open-api-bad-request.yml#L6

こうすると、リクエストの validation がスキップされます。

:::details 実行結果。

```console
$ runn run day18/open-api-bad-request.yml --debug
Run "Finds Pets by status" on "OpenAPIのSpecを参照し、Bad Requestのテストを行う場合".steps.findPetsByStatus
-----START HTTP REQUEST-----
GET /api/v3/pet/findByStatus?status=avaliable HTTP/1.1
Host: petstore3.swagger.io


-----END HTTP REQUEST-----
-----START HTTP RESPONSE-----
HTTP/2.0 400 Bad Request
Content-Length: 139
Access-Control-Allow-Headers: Content-Type, api_key, Authorization
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Content-Disposition
Content-Type: application/json
Date: Thu, 23 Nov 2023 06:10:57 GMT
Server: Jetty(9.4.9.v20180320)

{"code":400,"message":"Input error: query parameter `status value `avaliable` is not in the allowable values `[available, pending, sold]`"}
-----END HTTP RESPONSE-----
Run "test" on "OpenAPIのSpecを参照し、Bad Requestのテストを行う場合".steps.findPetsByStatus
.

1 scenario, 0 skipped, 0 failures
```

:::


OpenAPI の仕様書が存在する API をテストする際に、設定は必須だと感じています。
この API 仕様書のリクエストとレスポンスが仕様書通りになっているか？というのは API の品質を担保する上で重要になります。
筆者はこちらの機能によって、API の仕様バグを幾つか気づくことが何回かありました。 

