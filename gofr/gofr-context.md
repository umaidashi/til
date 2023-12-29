---
title: gofrのcontextを知る
date: 2023-12-22
hidden: false
tags:
---

[link](https://gofr.dev/docs/v1/references/context)

# すべての QueryParams を取得する

`http://localhost:8000/user?name=oidon&isStudent=true この API の場合

```go
ctx.Params()
```

↑ で、

```json
{
  "name": "oidon",
  "isStudent": "true"
}
```

が取得できる

> If the same key has multiple values then the value in c.Params will be comma-separated.

`http://localhost:8000/user?name=oidon&isStudent=true&name=umaidashi`
とすると

```json
{
  "name": "oidon,umaidashi",
  "isStudent": "true"
}
```

とカンマ区切りにしてくれる

# 任意の QueryParam を取得する

`http://localhost:8000/user?name=oidon&isStudent=true この API の場合

```go
ctx.Param("name")
```

で、

```json
"oidon"
```

が取れる

# パスから値を取得する

```go
app.GET("/user/{id}", func(ctx *gofr.Context) (interface{}, error) {
	...
})
```

api のエンドポイントに `{}` でパラメータを定義（今回は`id`）

```go
ctx.PathParam("id")
```

で`id`が取得できる

# リクエストボディの取得

1. ボディの型を定義

```go
type user struct {
	Name string `json:name`
	Age  int    `json:age`
}
```

2. BindStrict

```go
var u user
if err := ctx.BindStrict(&u); err != nil {
	return nil, err
}
```

アドレスを渡す必要がある（`&u`）の部分

`u.Name`, `u.Age` というように使える

# ヘッダー

`c.Header(<header_name>)` で取れる

```go
c.Header("Content-Type")
```

だと `application/json`
