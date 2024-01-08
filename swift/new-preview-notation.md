---
title: SwiftUIの新しいPreviewで変数定義する
date: 2023-1-8
---

## 概要

以前は以下のように書いていた

```swift
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

2023/6?くらいから以下のように変わったっぽい
楽に描けるようになった

```
#Preview {
  ContentView()
}
```

## viewに引数？を渡す場合

```swift
struct CardView: View {
    let scrum: DailyScrum
    var body: some View {
        Text(/*@START_MENU_TOKEN@*/"Hello, World!"/*@END_MENU_TOKEN@*/)
    }
}
```
このようなviewの場合（tutorialのscrumdinger)に、previewでは`CardView()`に`scrum`プロパティに値を渡す必要がある

> Missing argument for parameter 'scrum' in call


以下のように渡す

```swift
#Preview {
    CardView(scrum: DailyScrum.sampleData[0])
}
```

引数が増えると冗長になるため、その際は次のように変数宣言し、viewを`return`する（returnしないと怒られる）

```swift
#Preview {
    var scrum = DailyScrum.sampleData[0]
    return CardView(scrum: scrum)
}
```

引数定義したいな〜どうしようと詰まったが、あまり記事がなかったので
