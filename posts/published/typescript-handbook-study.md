---
title: "TypeScript handbook メモ"
created: "2021-10-05"
updated: "2021-10-24"
---

## Everyday Types

<https://www.typescriptlang.org/docs/handbook/2/everyday-types.html>

オプション引数を扱う時は、そのオプション引数が渡される場合/渡されない場合の条件分岐を書いておく必要がある。その時、undefined を使う場合もあれば、プロパティ名+?で対象となるプロパティにアクセスする方法もある。

```JavaScript
function printName(obj: { first: string; last?: string }) {

  if (obj.last !== undefined) {
    console.log(obj.last.toUpperCase());
  }

  console.log(obj.last?.toUpperCase());
}
```

### union

union type を使う場合、型の違いによって異なる処理をする場合がある。このとき、typeof や Array.isArray()など、対象の型判定をする関数を使って条件分岐させる。

```JavaScript
function printId(id: number | string) {
  if (typeof id === "string") {
    // In this branch, the type of id is 'string'
    console.log(id.toUpperCase());
  } else {
    // Here, the type of id is 'number'
    console.log(id);
  }
}
```

### Type Assertions

TypeScript が知らない型情報をプログラマが持っている場合、as や<>を使って明示的に型情報を付与する行為を Type Assertion と呼ぶ。

```JavaScript
// the programmer knows that the page will
// always have an HTMLCanvasElement with a given ID
const myCanvas =
  document.getElementById("main_canvas") as HTMLCanvasElement;
```

本来の型情報と異なる型が代入できないという問題は、二段階の type assertion を使って回避できる。

```JavaScript
// error
const x = "hello" as number;
// no error
const y = "hello" as any as number;
```

<!-- null and undefined -->

## Narrowing

関数の引数として渡される値の型によって、以降の処理を分岐させるのはよくあるパターンである。その分岐を処理するための方法には、さまざまなものがある。

### typeof type guards

JS の typeof 演算子を使って、条件分岐をさせる方法。わざわざ TS で書く必要はないように感じるが、typeof 演算子は歴史的経緯により null も object と表現してしまうので、コメントがないと入力値が Array や Function と null との区別ができないという問題がある。よって、TS で入力値として null を明示的に指定することには一定の意義がある。

```JavaScript
function printAll(strs: string | string[] | null) {
  if (typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  } else {
    // do nothing
  }
}
```
