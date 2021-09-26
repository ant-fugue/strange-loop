---
title: "JavaScript30, 1st week"
date: "2021-09-24"
---

HTML と CSS、特に CSS についての知識不足を最近痛感しているので、一度じっくりと基本を叩き込むために、[このレポジトリ](https://github.com/wesbos/JavaScript30)のソースコードをみっちり分析して、何も見ずに再現できるくらいにまでやっていく。

## [1st day](https://github.com/wesbos/JavaScript30/tree/master/01%20-%20JavaScript%20Drum%20Kit)

キーボードで対応するキーを押すと、画面上の対応する要素が光って音が鳴るアプリ

### ポイント

- css の値の相対指定を多用している
  - key のグループに対して flexbox を利用することで、等幅での配置を実現している
  - vh,rem など、root element のフォントサイズを参照する単位が多用されている
- ユーザーがキーを押したタイミングで、対応する要素にクラスが付加され、特定のアニメーションや効果を付与している

### 補足

rem は root element のフォントサイズに対する相対指定なので、html 要素に対する指定が 10px だったら、.4rem は 4px になる。個々に絶対値で指定するよりは、リファクタリングしやすいコードになりそうだけど、デメリットもあるのだろうか。

playSound()関数では、ユーザーが押したキーコードの読み取りに KeyBoardEvent の KeyCode プロパティを利用しているようだが、[現在は Deprecated な仕様になっている。](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode)

### 参考

[Web Audio API の学習](https://developer.mozilla.org/ja/docs/Web/API/Web_Audio_API)

[Document API の学習](https://developer.mozilla.org/ja/docs/Web/API/Document)

[CSS で使われる単位の整理](https://www.freecodecamp.org/news/css-unit-guide/)

## [2nd day](https://github.com/wesbos/JavaScript30/tree/master/02%20-%20JS%20and%20CSS%20Clock)

現在時刻をリアルタイムで表示するアナログ時計アプリ

### ポイント

Date コンストラクタで現在時刻を取得し、それを元に秒針・分針・時針の位置を更新して描画する関数を、setInterval で毎秒呼び出している

上記の関数は、具体的には取得した秒・分・時をもとに対応する針の回転角度を決め、その回転角度を CSS の transform プロパティの値として埋め込んでいる

### 補足

React とは異なり、vanillaJS を使って書く場合は、ドキュメントのノードに直接値を書き込んだり、再代入をしていくので、自分がページ上のどの要素を触っているかについて、devtools やデバッガーを使ってよく確認しながら書く必要がありそう。vscode だったら js でも変数の型はきっちり推測してくれるけど、他のエディタだったら、非常に混乱しそうだな。

### 参考

[Date オブジェクト](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
