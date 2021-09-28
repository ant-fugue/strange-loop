---
title: "'A mostly complete guide to error handling in JavaScript'を読んだ"
created: "2021-09-03"
---

普段、JS で同期的な処理を書いているときは、if 文でエラー処理を書いてから本体のコードを書くという形でエラー処理を実装している。しかし、Promise や async/await などの非同期処理が入ってきたときにどうエラー処理を書けばいいのかということについてよくわかっていなかった。そいで色々調べていくうちに、JavaScript におけるエラー処理を網羅してくれている[素晴らしい記事](https://www.valentinog.com/blog/error/)を見つけた。（[日本語版](https://zenn.dev/yukiota/articles/cb53ea21d7cf3994861a)はこちら）

## 概要

- throw されたものが例外になる
- エラーオブジェクトは throw されたときに例外になる
- 例外がキャッチされなかった場合、プログラムはクラッシュする
- 同期的処理の場合は、try/catch/finally で例外をキャッチできる
- 関数内で非同期に例外を投げる場合、try/catch/finally の処理は同期的なので、そちらが先に実行されてしまい、例外をキャッチできずにプログラムがクラッシュしてしまう
- つまり、非同期処理コードの実行中に投げられた例外は 外側でキャッチすることができない
- しかし、Promise ではリジェクトされた Promise を catch に処理させることができるため、try/catch を使わなくても、then/catch/finally を使って例外をキャッチして正しく処理することができる
- async/await は Promise を背後で利用しており、async 関数内で例外を投げることは、裏側の Promise をリジェクトすることと等価
- つまり、then/catch/finally を使って処理することが可能
- さらには、async/await の場合 try/catch/finally を使って、同期的処理と共通の syntax で例外処理が書ける

## 感想

try/catch が、setTimeout を使って投げれる例外をキャッチできない理由は、なるほどーと思った。JS の event loop の仕組みを知らないと全くわからんよね。

自分がこれまで try/catch を使うようなコードをほとんど書いてこなかったのは、人と一緒に開発したり、人のプログラムをじっくり読んだり、人が利用するようなプログラムを全然書いてこなかったからなのかなと反省した。

ともかく、これで明日から多少は自信を持って async/await を使ったプログラムを書けそうだ。

## 参考

JavaScript の Error オブジェクトの詳細は[こちら](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors)
