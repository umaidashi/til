---
title: 擬似クラス「:where」を使ってcssの詳細度を下げる
date: 2024-04-07
hidden: false
tags:
---

pr: [Fixed selected date text color not white in Calendar](https://github.com/yamada-ui/yamada-ui/pull/967)

##　修正したバグ

`yamada-ui`のカレンダーコンポーネントで、週末(`data-weekend`)や前月来月の部分(`data-outside`)にdata属性でスタイルを変えることができる。
しかし、ライトモードにおいて選択された状態にあたるdata属性(`data-selected`)と詳細度が一致し、プログラムの構成上後に記述される`data-weekend`が適用され、日付選択時に白字になっていない

```css
/* 0.1.0 lose */
[data-selected] {　
  color: white;
}

/* 0.1.0 win!! */
[data-weekend] {
  color: blue;
}
```
選択時は白字になって欲しいが、同じ詳細度なので後から適用される`data-weekend`が勝つ
（ナイトモードでは、ナイトモード用のスタイルで詳細度が上がるため問題が発生しない）

## 策1：詳細度を上げる（不採用）

今回、`data-selected`のスタイルを適用したいので、複合セレクタなどを使って詳細度を上げることで問題を解決できる

> 複合セレクタ
> `&[data-selected]&[data-selected]`とすることで、二重で適用され詳細度が2倍になる

```css
/* 0.2.0 win!! */
[data-selected][data-selected] {　
  color: white;
}

/* 0.1.0 lose */
[data-weekend] {
  color: blue;
}
```

しかし、`data-selected`属性は、yamada-uiの`core`パッケージにあたり、`calendar`パッケージ以外に影響する可能性があるため、今回は不採用とした

## 策2：`data-selected`を後に適用されるようにする（不採用）

同じ詳細度なので、後から適用されるようにすればよい
しかし、`core`パッケージの上に`calendar`パッケージが載っているので、`data-selected`を後に適用するのは難しい

## 策3：詳細度を下げる（採用）

詳細度の問題を解決したいとき、通常であれば詳細度を上げる対応が一般的（だと思っている）が、`where`擬似クラスを用いることで詳細度を下げることもできる

`:where()`は詳細度を0に保つことができるため、上書きしやすいときに便利

```css
/* 0.1.0 win!! */
[data-selected] {　
  color: white;
}

/* 0.0.0 lose */
:where([data-weekend]) {
  color: blue;
}
```
