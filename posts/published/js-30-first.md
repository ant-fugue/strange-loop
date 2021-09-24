---
title: "JavaScript30, 1st day"
date: "2021-09-24"
---

HTML と CSS、特に CSS についての知識不足を最近痛感しているので、一度じっくりと基本を叩き込むために、[このレポジトリ](https://github.com/wesbos/JavaScript30)のソースコードをみっちり分析して、何も見ずに再現できるくらいにまでやっていく。今回は[第一日目の課題](https://github.com/wesbos/JavaScript30/tree/master/01%20-%20JavaScript%20Drum%20Kit)。

## 概要

キーボードで対応するキーを押すと、画面上の対応する要素が光って音が鳴るアプリ

## ポイント

- css の値の相対指定を多用している
  - key のグループに対して flexbox を利用することで、等幅での配置を実現している
  - vh,rem など、root element のフォントサイズを参照する単位が多用されている
- ユーザーがキーを押したタイミングで、対応する要素にクラスが付加され、特定のアニメーションや効果を付与している

## 感想と考察

rem は root element のフォントサイズに対する相対指定なので、html 要素に対する指定が 10px だったら、.4rem は 4px になる。個々に絶対値で指定するよりは、リファクタリングしやすいコードになりそうだけど、デメリットもあるのだろうか。

playSound()関数では、ユーザーが押したキーコードの読み取りに KeyBoardEvent の KeyCode プロパティを利用しているようだが、[現在は Deprecated な仕様になっている。](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode)

### future work

[Web Audio API の学習](https://developer.mozilla.org/ja/docs/Web/API/Web_Audio_API)

[Document API の学習](https://developer.mozilla.org/ja/docs/Web/API/Document)

[CSS で使われる単位の整理](https://www.freecodecamp.org/news/css-unit-guide/)
