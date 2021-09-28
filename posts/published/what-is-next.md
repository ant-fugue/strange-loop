---
title: "NextのPageとrouting"
created: "2020-09-11"
---

このブログは、Next.js で作られているが、公式チュートリアルで作るブログをほぼそのまま持ってきただけで、Next に対する理解は大して深まっていない。
そこで、Next の概要、なかでも Page と Routing にフォーカスを当てて、[公式ドキュメント](https://nextjs.org/docs/getting-started)を読んでみた。

## page

- pages は"pages"ディレクトリにあるファイルからエクスポートされた、React Component である
- pages/about.js <-> /about
- dynamic routing に対応している->動的にページを指定できる

SSG
外部データにコンテンツが依存している
ページがレンダリングされる前にデータがを取得している必要がある: getStaticProps
外部データに Path が依存している: getStaticPaths
