---
title: aliasでコンパイル&runを効率化
date: 2024-02-03
hidden: false
tags:
---

C 言語を勉強している

C 言語で書かれたコードを実行するには、`gcc`でコンパイルして、実行する必要がある

```
$ gcc sample.c

$ ./a.out
```

↑ を毎回やるのがめんどくさい

## alias で楽する

`.zshrc`に次を追加する

```c
function runc () {
  gcc -O $1; ./a.out
}
alias -s c=runc
```

これは`suffix alias`で、`.c`で終わるときに実行される

例えば、`sample.c`と打つと、コンパイルされて`./a.out`を実行する

```c
alias c='(){ gcc -o $1 $1.c && ./$1}'
```

これは、`c sample`と打つと、`sample.c`がコンパイルされ、同じ`samplle`という名前の実行ファルが生成され、実行される
