## suitebook

* 利用規約: https://squeeze.notion.site/suitebook-API-8e07eabdfdb04c259d9dc958efa9325d
* お問い合わせ先: Support Team - support@suitebook.io

### API

#### Request Header Formats

以下を参考にリクエストヘッダに値をセットして、suitebook APIをご利用ください。

| Header        | Header Value                                                              |
|---------------|---------------------------------------------------------------------------|
| Content-Type  | `application/json`                                                        |`
| Accept        | `application/json`                                                        |
| User-Agent    | `Partner Test App` (suitebook APIを利用するアプリケーション名)                          |
| Authorization | `Key 5QGcvr4m4x8iWwXkG3B47TKQaokaRYVu5P0VneMH`（SQUEEZEが発行するAPIキー・APIトークン） |


### Authorization

Authorizationリクエストヘッダに必要なAPIトークンは、API利用規約を結んだ後にSQUEEZEで発行いたします。

APIトークンは、アプリケーション毎に異なるトークンを使用することができます。 またAPIトークンが漏洩した場合は、SQUEEZE側で該当トークンを無効にする対応をいたします。


### API Docs

* [version 1](https://squeeze-inc.github.io/docs/suitebook.io/api/v1/)
* [version 2](https://squeeze-inc.github.io/docs/suitebook.io/api/v2/)


### Webhook

* [version 1](https://squeeze-inc.github.io/docs/suitebook.io/webhook/v1/)
