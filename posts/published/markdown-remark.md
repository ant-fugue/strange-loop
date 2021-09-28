---
title: "ブログのマークダウンをGFM対応にした"
created: "2021-09-10"
updated: "2021-09-10"
---

本ブログの記事は、全て Vercel にホストした Next.js 上にマークダウンで書かれている。.md から.html を生成するパーサには remark を使っているが、記事内で表を使いたくなったので、マークダウン拡張仕様の GFM を導入した。

## 対象

Next.js の公式チュートリアルをなぞってブログを作った人

## 参考

[Next.js のための Remark / Rehype 入門](https://qiita.com/sankentou/items/f8eadb5722f3b39bbbf8)

[GitHub Flavored Markdown は何であって何でないか](https://qiita.com/tk0miya/items/6b81e0e4563199037018)

## 導入

[公式ドキュメント](https://github.com/remarkjs/remark-gfm)にあるように、まずは次のコードで必要なパッケージをダウンロードする。

```
npm install remark-parse remark-gfm remark-rehype rehype-stringify
```

その次に、[Next.js 公式チュートリアルでのマークダウン処理](https://nextjs.org/learn/basics/dynamic-routes/render-markdown)を次のように書き換える。

```
export async function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`);
  const fileContents = fs.readFileSync(fullPath, "utf8");

  // Use gray-matter to parse the post metadata section
  const matterResult = matter(fileContents);

  // Use remark to convert markdown into HTML string
  const processedContent = await unified()
    .use(remarkParse)
    .use(remarkGfm)
    .use(remarkRehype)
    .use(remarkStringify)
    .process(matterResult.content);
  const contentHtml = processedContent.toString();

  // Combine the data with the id and contentHtml
  return {
    id,
    contentHtml,
    ...matterResult.data,
  };
}
```

## 何をしているか

1. remarkParse:
   マークダウンの文字列をパースして AST に変換
2. remarkGfm:
   remark のプラグインで、パーサが Gfm を扱えるようにする
3. remarkRehype:
   マークダウンの AST を html の AST に変換
4. remarkStringify:
   html の AST から HTML 文字列を生成

元のチュートリアルでは、1,2 を remark で、3,4 を remark-html というプラグインで置き換えているので、元のチュートリアルにそのまま remarkGfm を突っ込んでもエラーになってしまう。
