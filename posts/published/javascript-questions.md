---
title: "JavaScript questions"
date: "2021-09-25"
---

## 概要

[javascript-questions](https://github.com/lydiahallie/javascript-questions)の深掘りしたいトピック

## Q1, Q2: var,let,const

| items         | const           | let             | var       |
| :------------ | :-------------- | :-------------- | :-------- |
| redeclaration | no              | no              | yes       |
| reassignment  | no              | yes             | yes       |
| scope         | block           | block           | function  |
| hoisting      | reference error | reference error | undefined |

Q1 は hoisting に関連する問題。「宣言なくして初期化なし」が let と const の特徴である。

Q2 は JS の実行モデルが関わってくるのでややこしいが、要するに関数スコープとブロックスコープの違いを問うている。
下のスクリプトにおいて、test1()の var で宣言された変数 x は、関数内で有効なので 1 を出力するが、test2()の let で宣言された変数 x は、{}内でのみ有効なので console.log(x)はエラーとなる。

```JavaScript
function test1() {
  if (true) {
    var x = 1;
  }

  console.log(x);
}

function test2() {
  if (true) {
    let x = 1;
  }
  console.log(x);
}

test1();
test2();
```
