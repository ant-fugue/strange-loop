---
title: "JavaScript questions"
created: "2021-09-25"
updated: "2021-10-24"
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

[アロー関数の概要](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## Q5

ブラケット記法とドット記法の違いに関する問題。
ドットの後に続くプロパティが、その実態は string であるのは考えてみると初学者にとってはややこしい。

[JavaScript Quickie— Dot Notation vs. Bracket Notation](https://codeburst.io/javascript-quickie-dot-notation-vs-bracket-notation-333641c0f781)

## Q6

JS において、あるオブジェクトに他のオブジェクトを代入する場合、参照渡しになるという例。両者ともに同じメモリを参照しているので、一方で値を変更したら、他方の値呼び出し結果も変わる。

プリミティブ型の変数に関しては、値渡しが適用される。

[データ型一覧](https://developer.mozilla.org/ja/docs/Web/JavaScript/Data_structures)

[値渡しと参照渡し](https://qiita.com/migi/items/3417c2de685c368faab1)

## Q8

クラスからインスタンスを new 演算子で生成した時、クラスで static に定義されたメソッドの扱いがどうなるのか、という問題。

new 演算子は組み込みやユーザ定義のクラス(より正確にはオブジェクト型)から、メソッドやプロパティを継承したインスタンスを生成する機能である。

ただし、クラスにおいて、static で定義されたメソッドやプロパティは、new で生成したインスタンスに渡されない

この問題では、最後の行の`console.log(freddie.colorChange('orange'));`によって、インスタンスには継承されない static なメソッドである colorChange にアクセスしようとしているので、エラーになる。

freddie インスタンスが colorChange を継承していないのは、`"colorChange" in freddie`の返り値が undefined になることによっても確かめられる

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

## Q17

タグ付きテンプレートリテラルの挙動の問題。

- テンプレートリテラルの特徴
  - ソースの中に挿入された改行文字は、すべてテンプレートリテラルの一部になる(\n がいらない)
  - 本問のように、関数名+テンプレートリテラル文字列の組み合わせをタグ付きテンプレートリテラルとよび、関数の最初の引数が文字列の配列となり、残りの引数が式に渡される

[テンプレートリテラル (テンプレート文字列)](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Template_literals)

## Q18

==と\===の違いを問う問題。型変換した結果での等値性を調べる必要がある場面はほぼないので、\===をいつも使っている。あとは Object.is()も等値性比較に使えるが、これでないといけない場面はまだ出会ったことがない。Jest や Chai などのユニットテストライブラリの内部実装とかで使われているんだろうか？

[等価性の比較と同一性](https://developer.mozilla.org/ja/docs/Web/JavaScript/Equality_comparisons_and_sameness)

## Q19

Rest parameters(残余引数)についての問題。Rest parameters は、不定数の引数を配列として関数に渡し、処理するために使われる。同様の使い方をするための仕組みとして、ES6 以前は arguments オブジェクトが使われていた。両者の違いは、それが配列であるか否かである。

```JavaScript
function getAge(...args) {
  // typeofでは同様の値を表示するが
  console.log(typeof arguments);
  console.log(typeof args);

  // argumentsは配列ではない
  console.log(Array.isArray(args));
  console.log(Array.isArray(arguments));
  console.log(Array.isArray(Array.from(arguments)));
}

getAge(21);
```

[残余引数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/rest_parameters)

## Q20

Strict モードの挙動に関する問題。Strict モードで変更される挙動は多岐に渡るが、大雑把にまとめると、次のような特徴がある。

- JavaScript モジュールでインポートされたモジュールは自動的に Strict モードになる
- 過去に宣言のなかったグローバル変数への代入は ReferenceError になる
- 通常のモードでは暗黙に失敗するミス（主に代入に関連するもの）を、例外を発生させるエラーとして扱う
- eval や arguments など、扱いの難しい関数の機能や影響範囲を制限する
- 将来の ECMAScript で導入される可能性のある予約語を、変数や引数の名前として使えなくする

<!-- ## Q21

名前解決の問題
eval()の回避策 -->

## Q22

JS の Storage API についての問題。
セッションストレージのデータは、ブラウザのタブが閉じられるときに消去されるが、ローカルストレージは明示的に削除されない限り、保存され続ける。これは、サーバにストレージの情報が保存されていて、ブラウザが再接続するときにそこから http ヘッダでストレージ情報を受け取るということなのだろうか？
<https://developer.mozilla.org/ja/docs/Web/API/Storage>

Storage API は一見便利そうだけれども、[この記事](https://www.rdegges.com/2018/please-stop-using-local-storage/)をみると、ごく簡単でユーザ登録が不要なアプリを除き、使用は避けた方が良さそう。

## Q25

重複する key がオブジェクトに存在する場合、対応する値には何が選ばれるか、という問題で、最後に追加された値が選ばれる。これは多くの場合、同様の値が存在することを知らずに追加してしまうというケースだと予想されるので、避けたい挙動のはず。TypeScript を導入するしかエラー検知の方法はないのか？
