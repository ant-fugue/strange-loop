---
title: "JavaScript questions"
created: "2021-09-25"
---

## 概要

[javascript-questions](https://github.com/lydiahallie/javascript-questions)の深掘りしたいトピック

## Q1,Q2

var と let の違いについての問題。

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

## Q3

ordinary function と arrow function の違い。
arrow function は this との結びつきを持たないので、たとえば shape 内に`thisRad: () => this.radius`というメソッドを定義し、それを呼び出した場合、this.radius はオブジェクト内の radius を参照せず、undefined が返ってくる。

他の違いはつぎのようなもの。

[参考](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

- this や super への結びつけを持たないので、メソッドとして使用することはできない
- arguments や new.target キーワードがない
- call、apply、bind のような、一般にスコープの設定のためのメソッドとは併用できない
- コンストラクターとして使用することはできない(new keyword を使ったオブジェクト生成に利用できない)
- 本体内で yield を使用することはできない

## Q5

ブラケット記法とドット記法の違いに関する問題。
ドットの後に続くプロパティが、その実態は string であるのは考えてみると初学者にとってはややこしい。
<https://codeburst.io/javascript-quickie-dot-notation-vs-bracket-notation-333641c0f781>

## Q6

JS において、あるオブジェクトに他のオブジェクトを代入する場合、参照渡しになるという例。両者ともに同じメモリを参照しているので、一方で値を変更したら、他方の値呼び出し結果も変わる。

プリミティブ型の変数に関しては、値渡しが適用される。

<https://developer.mozilla.org/ja/docs/Web/JavaScript/Data_structures>
<https://qiita.com/migi/items/3417c2de685c368faab1>

##
