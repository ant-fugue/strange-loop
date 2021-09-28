---
title: "はじめてのCSSスタイリング"
created: "2021-09-13"
---

これまで、CSS はほぼコピペするだけで、自分でほとんど書いたことがなかったので、ごく簡単にその変遷をまとめた。単純に[このポッドキャスト](https://anchor.fm/basuke-suzuki/episodes/CSS-with-kara_d-e15ph61)が面白く、興味を持ったというのもある。

**参考**
[色々書き比べた結果 Tailwind CSS にしたという話](https://qiita.com/Takazudo/items/78ee15564bfefdea844c)
[それでも私が Tailwind CSS ではなく、CSS Modules を推す理由](https://qiita.com/70ki8suda/items/b95aeb4d4d3cab57a8fe)
[BEM introduction](http://getbem.com/)

**React 以前(SPA 以前？)**

- 各 HTML ごとに対応する CSS が存在
- よって、それぞれの HTML ファイル内ごとに、変数名の衝突がないように CSS の要素を命名しなければならない
- CSS の命名の混乱を避けるため、各 HTML 要素に Block, Element, Modifier で構成されるクラス名を定義する BEM という命名規則が考案され、広く使われた
- 一方で、一意な特定を可能にするため、CSS のクラス名はどうしても冗長になる

**React 以後**

- HTML に含まれる要素を、js コンポーネントの組み合わせとして構成できるようになった
- しかし、css でスタイルをどう適用するかは様々な実装が乱立している

**CSS modules**

- コンポーネントと同名の css ファイル(コンポーネント名が layout.js なら layout.module.css)を作成し、module.css で指定されたクラスにランダムな文字列を足してクラス名にすることで、コンポーネント内でスコープを作ったような状態にできる
- よって、各コンポーネントごとに閉じた形でスタイルを適用できて、しかもグローバル css との名前衝突の問題もない

**使える vscode 拡張**

CSS モジュールで定義されたクラスを参照できる拡張。
[CSS Modules](https://marketplace.visualstudio.com/items?itemName=clinyong.vscode-css-modules)

React や Vue が使えない環境だったら、これは重宝するのかな？
[BEM Helper](https://marketplace.visualstudio.com/items?itemName=Box-Of-Hats.bemhelper)
