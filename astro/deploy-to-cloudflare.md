---
title: Cloudflare Pages にデプロイする
date: 2023-12-29
hidden: false
tags:
---
## install wrangler

```
npm install wrangler
```

## wangler login

```
npx wrangler login
```
ブラウザで認証してください

## Deploy

```
npx wrangler pages deploy dist
```

`dist`ディレクトリにビルドされたファイルがデプロイされる
