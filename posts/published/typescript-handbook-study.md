---
title: "TypeScript handbook メモ"
created: "2021-10-05"
updated: "2021-10-28"
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

### Truthiness narrowing

if 文の条件節で、true/false に変換される値を使って条件分岐をする方法。先程の printAll()関数は、たとえば次のように書き換えられる。

```JavaScript
function printAll(strs: string | string[] | null) {
  // utilize the fact that not empty string is coerced to true
  // eliminate the possibility of null
  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```

しかし、これは入力値が null の場合をどう管理するかということを考えると、他に良い解決策がありそう。引数の型の null 自体を削除するか、あるいは次のようにはじめで null の値をはじいてしまうとか。

```JavaScript
function printAll(strs: string | string[] | null) {
  if (!strs) {
    console.log("null!!");
  }

  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```

### instanceof narrowing

JS の instanceof 演算子を使って条件分岐をする方法。Handbook にも書かれている通り、クラスとそこから new で生成するオブジェクトの種類のよって処理を分岐させたいときに使うのだろう。

```TypeScript
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());
  } else {
    console.log(x.toUpperCase());
  }
}

logValue("this");
logValue(new Date("1995-12-17T03:24:00"));
```

### using type predicates

JS の演算子ではなく、TS の機能を直接使って型による条件分岐をしたいときは、返り値に type predicates を指定して、どんな型が帰ってくるのかを明示的に指定する方法が使える。

```TypeScript
interface Fish {
  swim: true;
}

interface Bird {
  sing: true;
}

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

console.log(isFish({ swim: true }));
```

### Exhaustiveness checking

never を使ったエラーハンドリング

<!-- https://qiita.com/macololidoll/items/1c948c1f1acb4db6459e -->

```TypeScript
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}

interface Triangle {
  kind: "triangle";
  sideLnegth: number;
}

type Shape = Circle | Square | Triangle;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      // defaultでcircleとsquare以外の可能性はneverとされている -> それ以外の可能性は存在しない
      // よって、Triangleという型を持つ変数が存在する可能性もないはずなので、エラーが出る
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
  }
}
```

## More on Functions
