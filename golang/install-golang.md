---
title: Install Golang
date: 2023-12-21
hidden: false
tags:
---

# Download from `go.dev`

[here](https://go.dev/dl/)

macbook pro 14inch (m1 pro) を使用しているので、`Apple macOS(arm64)`をダウンロード

# Install

ダウンロードしたやつを開く

# PATHを通す
```txt:.zshrc
export PATH="/usr/local/go/bin:PATH"
```

## 確認
terminalを再起動して、`go version`
```
go version
> go version go1.21.5 darwin/arm64
```

いいかんじ