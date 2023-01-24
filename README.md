## suitebook

* 利用規約: https://squeeze.notion.site/suitebook-API-8e07eabdfdb04c259d9dc958efa9325d
* お問い合わせ先: Support Team - support@suitebook.io

### Docs

* [version 1](https://squeeze-inc.github.io/docs/suitebook.io/v1/)
* [version 2](https://squeeze-inc.github.io/docs/suitebook.io/v2/)

### API

#### Request Header Formats

以下を参考にリクエストヘッダに値をセットして、suitebook APIをご利用ください。

| Header        | Example Value                                  | Description                 |
|---------------|------------------------------------------------|-----------------------------|
| Content-Type  | `application/json`                             ||
| Accept        | `application/json`                             ||
| User-Agent    | `Partner Test App`                             | suitebook APIを利用するアプリケーション名 |
| Authorization | `Key 5QGcvr4m4x8iWwXkG3B47TKQaokaRYVu5P0VneMH` | SQUEEZEが発行するAPIキー・トークン      |

#### Authorization

Authorizationリクエストヘッダに必要なAPIトークンは、API利用規約を結んだ後にSQUEEZEで発行いたします。

APIトークンは、アプリケーション毎に異なるトークンを使用することができます。 またAPIトークンが漏洩した場合は、SQUEEZE側で該当トークンを無効にする対応をいたします。

### Webhook

#### Endpoint

SQUEEZEに、Webhook通知先のエンドポイントをお知らせください。

セキュリティのために、Webhook通知先のエンドポイントは `HTTPS` である必要があります。

#### Options

Webhook通知を購読するイベントとして

* 予約のみ
* 在庫のみ
* 予約・在庫の両方

のいずれかを設定できます。

また通知されるデータ形式として、[version 1](https://squeeze-inc.github.io/docs/suitebook.io/v1/)
または [version 2](https://squeeze-inc.github.io/docs/suitebook.io/v2/) のどちらかを選んでください。

#### Authorization

Webhookではシグネチャ認証を採用しています。

Webhookリクエストは、RSA証明書を使用して署名されています。SQUEEZE側から公開鍵を発行し、ヘッダ等の値を含めて共有いたします。

For example:

```
Authorization: Signature keyId="booking_web_hooks",algorithm="rsa-sha256",headers="host url method date content-type body",delimiter="|",signature="aVp9aJ/LB4uuZIbWPXMsK9EwMHt3I09VYHWkVIxUEZE27ysJ4nRkz3KbmlOTcipX/P7x0CWTepF2E3sNxn/96oHxA9BTwGMv+3ohGXItTHuZcqcWuWOF0uFXozWAZDf6S84ifCNqa6h/VyWcw8BnLEk3yYZH0VEbzfehaV8eGzO4d6RiGsTTfQmpN762lKbyJzWI5OXD4+/A2B/3SuPYFd1Y4ar5T+PtKL5H8tt4kYNEVXuEDG/PfgEXJ9fyJ/xX2BAlsxRqKwN0xPAui+KvBqnOEtkCuKb9/ylajgolCTXWOgEX0apKvmjAgcu4231Q2WQ4sQ99IyQTFjKzgZTz0w=="
```

| Key       | Example Value                                  | Description       |
|-----------|------------------------------------------------|-------------------|
| keyId     | `booking_web_hooks`                            | 固定値               |
| algorithm | `rsa-sha256`                                   | 固定値               |
| headers   | `$host $url $method $date $content-type $body` | 下記を参照             |
| delimiter | `&#124;`                                       | 固定値               |
| signature | `aVp9aJ/L ... gZTz0w==`                        | 上記のフィールドから計算したデータ |

##### headers

| Key          | Example Value                   | Description     |
|--------------|---------------------------------|-----------------|
| host         | `example.org`                   | 通知先エンドポイントのホスト名 |
| url          | `/foo`                          | 通知先エンドポイントの相対パス |
| method       | `POST`                          | 固定値             |
| date         | `Sat, 20 Jun 2015 00:47:57 GMT` | HTTPヘッダの内容      |
| content-type | `application/json`              | HTTPヘッダの内容      |
| body         | `{ "payload" : "test" }`        | リクエストの内容        |

クライアントはシグネチャを検証するために、まずheadersパラメータで指定された順序で、delimiterで区切られたペイロードを構成します。

このペイロードを、RSA証明書を使用して検証します。検証する前に、必ずBase64で署名をデコードしてください。

#### Response

Webhook通知を受け取ったら、10秒以内にHTTP 200番台のレスポンスを返してください。それ以外の場合は、リクエストは失敗したと見なされ再試行されます。

#### Retry

すべての通知は非同期であり、以下のように最大5回まで再試行されます。

| 実行回数          | 次のリトライまでの待ち時間 |
|---------------|---------------|
| 1回目           | 10秒後          |
| 2回目 (リトライ1回目) | 1分後           |
| 3回目 (リトライ2回目) | 5分後           |
| 4回目 (リトライ3回目) | 20分後          |
| 5回目 (リトライ4回目) | 3時間後　         |

