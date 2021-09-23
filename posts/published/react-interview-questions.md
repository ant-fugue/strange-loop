---
title: "React interview questions"
date: "2021-09-23"
---

[reactjs-interview-questions](https://github.com/sudheerj/reactjs-interview-questions)という GitHub レポジトリがある。スター数 16500 というかなり人気のレポジトリで、React,Redux,React Native など、React 関連の様々な Q&A を載せてくれている素晴らしいレポジトリである。

React についてより深く、詳しくなっていくためのとっかかりとして、このレポジトリを使っていく。

## 13. What is the difference between HTML and React event handling?

| items                    | HTML      | React     |
| :----------------------- | :-------- | :-------- |
| イベントの命名規則       | lowercase | camelCase |
| 関数呼び出し時の()の有無 | 有        | 無        |

## 24-26: Virtual DOM

実 DOM とは別に確保したメモリー上の DOM でデータ操作を行い、その結果として生じた実 DOM との差分をレンダリングの際に実 DOM に書き込む手法

(vanilla JS を使ったパターンとの比較　=> 別記事で執筆予定)

参考:
[What is DOM, Shadow DOM and Virtual DOM?](https://www.youtube.com/watch?v=7Tok22qxPzQ)
