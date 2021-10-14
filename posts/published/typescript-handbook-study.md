---
title: "TypeScript handbook メモ"
created: "2021-10-05"
updated: "2021-10-09"
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
    // In this branch, id is of type 'string'
    console.log(id.toUpperCase());
  } else {
    // Here, id is of type 'number'
    console.log(id);
  }
}
```
