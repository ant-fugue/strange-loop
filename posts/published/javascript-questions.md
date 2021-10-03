---
title: "JavaScript questions"
created: "2021-09-25"
updated: "2021-10-01"
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

### 参考

[アロー関数の概要](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## Q5

ブラケット記法とドット記法の違いに関する問題。
ドットの後に続くプロパティが、その実態は string であるのは考えてみると初学者にとってはややこしい。

### 参考

[JavaScript Quickie— Dot Notation vs. Bracket Notation](https://codeburst.io/javascript-quickie-dot-notation-vs-bracket-notation-333641c0f781)

## Q6

JS において、あるオブジェクトに他のオブジェクトを代入する場合、参照渡しになるという例。両者ともに同じメモリを参照しているので、一方で値を変更したら、他方の値呼び出し結果も変わる。

プリミティブ型の変数に関しては、値渡しが適用される。

### 参考

[データ型一覧](https://developer.mozilla.org/ja/docs/Web/JavaScript/Data_structures)

[値渡しと参照渡し](https://qiita.com/migi/items/3417c2de685c368faab1)

## Q8

クラスからインスタンスを new 演算子で生成した時、クラスで static に定義されたメソッドの扱いがどうなるのか、という問題。

new 演算子は組み込みやユーザ定義のクラス(より正確にはオブジェクト型)から、メソッドやプロパティを継承したインスタンスを生成する機能である。

ただし、クラスにおいて、static で定義されたメソッドやプロパティは、new で生成したインスタンスに渡されない

この問題では、最後の行の`console.log(freddie.colorChange('orange'));`によって、インスタンスには継承されない static なメソッドである colorChange にアクセスしようとしているので、エラーになる。

freddie インスタンスが colorChange を継承していないのは、`"colorChange" in freddie`の返り値が undefined になることによっても確かめられる

### 参考

[new 演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/new)

## Q11

プロパティやメソッドのプロトタイプ継承についての問題。これまで、prototype の仕様について、よく理解しないまま書いてきたので、この問題は学習のよいきっかけになった。

[MDN の記事](https://developer.mozilla.org/ja/docs/Web/JavaScript/Inheritance_and_the_prototype_chain#using_prototypes_in_javascript)によると、オブジェクトのメソッドにアクセスした場合、prototype をどんどんさかのぼっていき、それ以上先の prototype がない場合(null を返す場合)に、undefined を返すというメカニズムらしい。

この問題の場合、Person へのメソッド追加に prototype がないため、子オブジェクトには追加されたメソッドが継承されない。
そのことは、`"getFullName" in member`が false を返すことで確かめられる。

<!-- [オブジェクトのプロパティにアクセスする方法の一覧](accessing-js-properties) ->  -->

## Q12

new キーワードの有無で、インスタンスの挙動がどう変わるのかを問う問題。

## Q16

postfix な incremental 演算子と、 prefix な incremental 演算子の違いを問う問題。普段は postfix なバージョンを使うので、後者を使うべき場面というのが思いつかないな。。
