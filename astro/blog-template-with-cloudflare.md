---
title: AstroをC3で簡単に Cloudflare へデプロイした
date: 2023-12-23
hidden: false
tags:
---
# 手順
1.  Cloudflareアカウントを作る（メール認証まで済ませる）
2. `npm create cloudflare@latest`
	1. astroを選ぶ
	2. blogを選んだ
3. そのまま流れで`deploy`できる


# 注意点

そのままだと`dynamic routing`部分がエラーになる（SSGの設定が必要）
`post.render is not a function`

ビルドすると、以下のwarningが出てる
```zsh
[WARN] [router] getStaticPaths() ignored in dynamic page /src/pages/blog/[...slug].astro. Add `export const prerender = true;` to prerender the page as static HTML during the build process.
```

警告通り、`src/pages/blog/[...slug].astro`に以下を追加
```astro:src/pages/blog/[...slug].astro
---
+ export const prerender = true;
import { type Coller ....
```
ssgすることが明示的になる