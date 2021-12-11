---
title: "Clojureの基礎固め"
created: "2021-12-11"
updated: "2021-12-11"
---

ぼくにプログラミングの面白さ、深さ、自由さを教えてくれた Scheme もとい LISP。現在最も勢いがあり、商用のプロダクトに利用されている憧れの Clojure に入門することにした。まずは、[公式ドキュメント](https://clojure.org/guides/learn/syntax)をコツコツと読んでいく。

## basic types

### Numeric

- integer: 64bit 固定小数点数
- floating point: 64bit 浮動小数点数
- ratio: numerator と denominator の組み合わせ

### Character

- string: double qoutes で囲まれた文字
- character: バックスラッシュではじまる文字
- regular expression: #ではじまる文字

### Symbol

> Symbols are composed of letters, numbers, and other punctuation and are used to refer to something else, like a function, value, namespace, etc.

このように公式ドキュメントには書いてあるが、イメージがわかんな。どういうことだろう。

### Literal collections

- list
- vector
- set
- map
