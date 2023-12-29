---
title: cronスケジューラーで定期ビルド＆デプロイする
date: 2023-12-29
hidden: false
tags:
---
[参考](https://zenn.dev/mr_ozin/articles/2c5553196a906b)

## Why
- [ポートフォリオ](https://umaidashi.me/)を新しくした際に、tilをコンテンツとして配信し始めた
- tilは随時書いていくが、ポートフォリオは静的であるため、たまにビルドしなければコンテンツが更新されない
- 手動は辛いので、スケジューラーみたいなので自動化してみる

## webhookのurlを取得

cloudflare pagesでは、叩くとビルドが走るエンドポイントを提供してくれる
※git接続したプロジェクトではなければ利用できないので注意

`Workers & Pages > <PROJECT> > Settings > Builds & deployments > Deploy hooks > Add deploy hooks`
↑をクリックするとhooksを作れる

## Cron Triggerの作成

参考にした記事を参照ください