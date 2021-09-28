---
title: "記事の日付が全て同じになるエラーの修正"
created: "2021-09-13"
updated: "2021-09-13"
---

このブログのトップページで、なぜか全ての記事の日付が、最新の日付と時間で更新されるという不具合があったので、その原因を特定し、修正した。

エラーの原因は、index.js の中のこのプログラムにあった。

```JavaScript
<ul className={utilStyles.list}>
  {allPostsData.map(({ id, date, title }) => (
    <li className={utilStyles.listItem} key={id}>
      <Link href={`/posts/${id}`}>
        <a>{title}</a>
      </Link>
      <br />
      <small className={utilStyles.lightText}>
        <Date dateString={date} />
      </small>
    </li>
  ))}
```

これは、マークダウンで書かれた全ての記事を取得して、そのタイトルと日付をトップページに表示する処理である。この処理のなかで、map 内で渡される引数とそれを使うコンポーネントの型の不一致が問題を引き起こしていた。具体的には、

- TypeScript で定義された Date インターフェースは、Date オブジェクトを引数として string や number を返す
- map 内で渡された date プロパティは string である
- よって、Date コンポーネントは予期しない型を受け取る
- 結果、Date コンポーネントは Date コントラクタを呼び出して引数として受け取り、その結果を表示している

この仮説は、たとえば dateString の中身をこのように適当な文字列に書き換えても、現在の日付と時間が表示されることによって確かめられる。

```
<Date dateString={`hello`} />
```
