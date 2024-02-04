---
title: remoteのブランチをローカルにフェッチ
date: 2024-02-04
hidden: false
tags:
---

## 結論

```
$ git checkout -b <作成するブランチ名> <リモートのブランチ名>
```

## ローカルのブランチ確認

```
$ git branch
```

## リモートのブランチ確認

```
$ git branch -a
```

## 例

```
$ git checkout -b feat/hoge origin/feat/hoge
```
