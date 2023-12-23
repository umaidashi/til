---
title: ドメインをvercelからcloudflareに移した
date: 2023-12-23
hidden: false
tags:
---
### Why
> portfolioおよびブログをリニューアルしているところで、cloudflareのdnsを使うことにした
> vercelに登録していたドメインをcloudflareに移した

## VercelのDomainsから削除
[https://vercel.com/dashboard/domains](https://vercel.com/dashboard/domains) ←ここ

## Cloudflareで登録
1. `cloudflare / Website` へアクセス
2. `add a site` をクリックし、対象ドメインを登録
3. まだ登録終わってないのでエラーになってると思う（三角にびっくりマーク）

## ドメイン買ったところで設定しなおす
1. 今回は、github educationで無料でもらった、`.me`のドメインだったので、namecheap.comにアクセス
2. `Namecheap / Domain List / 対象ドメイン / Domainタブ / NAMESERVERS / Custom DNS` にて、`Cloudflare / website / 対象ドメイン / overview` にある、"Complete your nameserver setup"の"2. Replace with Cloudflare's nameservers"の`nameserver 1`と`nameserver 2`を登録

意外と反映早い

