---
title: Twitterシェアリンクを追加した
date: 2024-1-5
---

## 実装

### Logic

```ts
const TwitterAccountName = "umaidashi18";

const generateShareTweetUrl = (
  text: string,
  url: string,
  hashtags?: string[]
) => {
  const _url = new URL("https://twitter.com/intent/tweet");

  if (text !== undefined) {
    _url.searchParams.set("text", text);
  }
  if (url !== undefined) {
    _url.searchParams.set("url", url);
  }
  if (hashtags !== undefined) {
    _url.searchParams.set("hashtags", hashtags.join(","));
  }
  _url.searchParams.set("via", TwitterAccountName);

  return _url.href;
};

export default generateShareTweetUrl;
```

`undefined`にならないように、URL クラスを使用し、`searchParams`をセットしていく
型安全に url を作成できる

### UI

```astro
---
import socialIcons from "@assets/socialIcons";
import generateShareTweetUrl from "@utils/generateShareTweetUrl";
import LinkButton from "./LinkButton.astro";

type Props = {
  url: string;
  text: string;
  hashtags?: string[];
};
const { url, text, hashtags } = Astro.props;
const href = generateShareTweetUrl(text, url, hashtags);
---

<div class={`social-icon`}>
  <LinkButton href={href} className="link-button" title={text}>
    <Fragment set:html={socialIcons["Twitter"]} />
    Share
  </LinkButton>
</div>

<style>
  .social-icon {
    @apply flex-wrap justify-center gap-1;
  }
  .link-button {
    @apply p-2 hover:rotate-6 sm:p-1;
  }
</style>
```

[`astro-paper`](https://github.com/satnaing/astro-paper)を使用しているため、既存のコンポーネントを使った

### 使用例

```astro
---
...

const shareUrl = Astro.url.origin + Astro.url.pathname;
---
<ShareTweetButton text={title} url={shareUrl} />
```

ページの URL は、[`Astro.url`](https://docs.astro.build/ja/reference/api-reference/#astrourl)で取得することができる

## まとめ

いい感じになってよかった
