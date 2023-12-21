---
title: Gofrを試しに使ってみた
date: 2023-12-21
hidden: false
tags:
---


# setup
[get start guid](https://gofr.dev/docs/v1/quick-start/introduction)

- ディレクトリ作る
```
mkdir gofr-trial
cd gofr-trial
```

- 初期化
```
go mod init gofr-trial
```
[https://zenn.dev/optimisuke/articles/105feac3f8e726830f8c](https://zenn.dev/optimisuke/articles/105feac3f8e726830f8c)

- package追加
```
go get gofr.dev
```

- `main.go`に以下を追加
```go:main.go
package main

import "gofr.dev/pkg/gofr"

func main() {
    // initialise gofr object
    app := gofr.New()

    // register route greet
    app.GET("/greet", func(ctx *gofr.Context) (interface{}, error) {

        return "Hello World!", nil
    })

    // Starts the server, it will listen on the default port 8000.
    // it can be over-ridden through configs
    app.Start()
}
```

- 実行前に必要なモジュールをダウンロード
```
go mod tidy
```
> [不足していたり、使われていないモジュールに良しなに対応してくれるコマンドみたいですね。](https://qiita.com/wangqijiangjun/items/28037d06efe86ec8dd0f)

結構時間かかった（3mくらい）

- 実行！
```
go run main.go
```

[http://localhost:8000/greet](http://localhost:8000/greet)へアクセス

- 結果
```
{"data":"Hello World!"}
```
しっかりとれてる

# Gofrの特徴であるログ

```
go run main.go
WARN [17:45:27]  Failed to load config from file: ../../configs/.env, Err: open ../../configs/.env: no such file or directory
                 (Memory: 6383688 GoRoutines: 2)
WARN [17:45:27]  APP_VERSION is not set. 'dev' will be used in logs
                 (Memory: 6389272 GoRoutines: 2)
WARN [17:45:27]  APP_NAME is not set.'gofr-app' will be used in logs
                 (Memory: 6390872 GoRoutines: 2)
INFO [17:45:27]  Starting metrics server at :2121
                 (Memory: 6456080 GoRoutines: 2)
INFO [17:45:27]  GET /greet HEAD /greet GET /.well-known/health-check HEAD /.well-known/health-check GET /.well-known/heartbeat HEAD /.well-known/heartbeat
                 (Memory: 6461072 GoRoutines: 3)
INFO [17:45:27]  starting http server at :8000
                 (Memory: 6464208 GoRoutines: 4)
INFO [17:45:58]  Route GET / not found GET / - 0.65ms (StatusCode: 404)
  CorrelationId: 5c6f7ce09fdd11ee8c6932298911805b (Memory: 6610864 GoRoutines: 6)
INFO [17:45:58]  Route GET /favicon.ico not found GET /favicon.ico - 0.18ms (StatusCode: 404)
  CorrelationId: 5c8022b69fdd11ee8c6932298911805b (Memory: 6668664 GoRoutines: 7)
INFO [17:46:03]  Route GET /great not found GET /great - 0.21ms (StatusCode: 404)
  CorrelationId: 5f65a8669fdd11ee8c6932298911805b (Memory: 6710544 GoRoutines: 7)
INFO [17:46:08]  GET /greet - 0.17ms (StatusCode: 200)
  CorrelationId: 629450149fdd11ee8c6932298911805b (Memory: 6754592 GoRoutines: 7)
INFO [17:47:04]  GET /greet - 0.17ms (StatusCode: 200)
  CorrelationId: 8411756e9fdd11ee8c6932298911805b (Memory: 6793336 GoRoutines: 6)
```
しっかりでてますねえ


# main.goを読んでいく

[ここ](https://gofr.dev/docs/v1/quick-start/introduction#understanding-the-example)

## GoFr サーバーの作成
```go:main.go
app := gofr.New()
```
> フレームワークが初期化され、構成に基づいてデータベース接続プールの構成、ロガーの初期化、CORS ヘッダーの設定などのさまざまなセットアップ タスクが処理されます。

pythonの flask や fastAPI みたい

## ハンドラーをパスにアタッチする
> HTTP リクエストを特定のハンドラー関数に関連付ける

```
app.GET("/greet", HandlerFunction)
```

### Good To Know
>  ハンドラー関数は`func(ctx *gofr.Context) (interface{}, error)`シグネチャに従う必要があります。 これらはコンテキストを入力として受け取り、応答データとエラー (`nil`成功時に設定される) の 2 つの値を返します。
>  GoFr では、`ctx *gofr.Context`リクエスト、レスポンス、依存関係のラッパーとして機能し、さまざまな機能を提供します。

- インプットを`context`として扱い、
- `return`はデータとエラー（成功時は`nil`）を返す
↑を満たす必要がある

> GoFr では、`ctx *gofr.Context`リクエスト、レスポンス、依存関係のラッパーとして機能し、さまざまな機能を提供します。

flask, fastAPI と似てるが、引数とかheaderとかいろいろを自分でやる必要がなくて、contextでいろんな機能提供してくれるっぽい
詳細を次見てく

## サーバーの起動

> `app.Start()`が呼び出されると、提供された構成に基づいて HTTP サーバーとミドルウェアが構成され、開始されます。ヘルスチェックのルート、Swagger UI などの重要な機能を管理します。デフォルトのポート 8000 でサーバーを起動します。

swagger 入っているのか（あんま詳しくないけど）
ちょっと設定が必要らしい
[setup swagger](https://gofr.dev/docs/v1/references/swagger#swagger)
これも次やってく