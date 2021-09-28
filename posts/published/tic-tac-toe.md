---
title: "三目並べをReact+AWS amplifyで作っている"
created: "2020-09-09"
---

## 概要

数ヶ月前から勉強し始めた、React と AWS Amplify を使って三目並べアプリを作っている。
一般公開できるクオリティーにするにはもう少し時間が必要だが、一旦開発中のデモを載せつつ、その構成やこれまでの学びをまとめてみる。

三目並べは、[React 本家のチュートリアル](https://ja.reactjs.org/tutorial/tutorial.html)の内容をほぼそのまま持ってきて、それを TypeScript+関数コンポーネントにリファクタリングした（関数コンポーネント化については、[こちらのサイト](https://ja.reactjs.org/tutorial/tutorial.html)を参考にした）。そして、ユーザ認証と、プレイヤー成績を表形式で表示する機能を追加した。さらに、CPU の自動プレイ機能なども追加実装予定である。

## デモ

![デモ](/images/tic-tac-toe.gif)

## 基本構成

### テーブル構成

User:

ユーザープールから、クレデンシャルを除いたユーザ情報を登録しているテーブル
管理者を除き、ユーザは自分自身の情報しか取得できないようになっている

PlayerInformation:

プレイヤーのユーザ名と戦績を記録するためのテーブル
ユーザ名は、User テーブルから取得する

### エンドポイント

root:
Cognito を使って、サインアップとサインイン時の認証を行う
サインアップ時には、登録メールアドレスに verification code を送り、ユーザにそのコードを入力してもらうことで認証を完了させる
認証完了をトリガーとして、Lambda が Users table にユーザ情報を登録する
サインインは、ユーザ名とパスワードで行う

game:
ゲームの本体
ゲームスタート時とゲーム終了時に、プレイヤーの総ゲーム数と総勝利数を PlayerInformation テーブルに記録する

player-info:
全てのプレイヤーの名前、ゲーム数、勝利数が表形式で表示される
データは、PlayerInformation テーブルから GraphQL の query で取得される

### 使用技術

![システム構成図](/images/tic-tac-toe-system.png)

- React
- AWS Amplify
- AWS AppSync(graphQL)
- AWS Lambda
- Amazon Cognito
- Amazon DynamoDB

## 学びと感想

### React 面白いが難しい

このアプリを作るまで、React にほとんど触ったことがなかったので、非常に勉強になった。
特に、immutability の担保を大原則として、全体が設計されているのは、非常に美しいと思う。
その一方で、学習コストはかなり高く、特にライフサイクルへの不十分な理解に起因するエラーを何度も踏んでいるので、もっとじっくり腰を据えて勉強したい。

### AWS Amplify の便利さと設定の大変さ

AWS Amplify Cli を使うと、ユーザ認証と認証画面の実装、GraphQL のスキーマ定義と連動する DynamoDB テーブルの定義、さらにはスキーマと連動するクエリの自動生成など、面倒なことを全部 Amplify 側でやってくれて、しかもそのほとんどがプロトタイプ用途では無料枠に収まるので、至れり尽くせりという感じだった。

その一方、各サービスの設定項目は膨大で、自分のユースケースにあてはまるものが何かをスッと見つけるのは難しく、公式ドキュメントを読んでも、自分が知りたいことが Amplify と Lambda や AppSync などの各リソースのどちらのドキュメントに書いてあるのがわからず、情報を探すのに非常に苦労した。そんななか、[AWS で AWS Amplify の開発エンジニアをつとめていたというこの方の動画](https://www.youtube.com/c/naderdabit)は非常に参考になった。

### GraphQL は便利

今回、AWS AppSync を使ってはじめて GraphQL を実装し、そのわかりやすさに感銘した。
AWS AppSync console から、クエリを投げて簡単にスキーマの正しさを検証できるし、バックエンドの実装を気にせずにフロントエンドを実装していくことができた。
もちろん、クエリの階層が深くなっていくと計算量がやばくなっていくことは容易に想像できるが。

### JavaScript のイベントループがようやくちょっとわかった

Game コンポーネント内で、プレイヤーがゲームを開始する時点でプレイヤー情報の登録あるいは更新を行うクエリおよびその返り値と、Game コンポーネント内で利用する state をやりとりする処理を書いたのだが、タイミングの問題で state が更新されない問題が生じ、あまりわかっていなかった Promise および async/await について勉強し直した。結果として、JavaScript のイベントループが次のようなものであることをようやく理解した。

- 同期的なコードを call stack に積む
- Promise, async/await による非同期なコード実行は、job queue に登録される
- setTimeout など、非同期でイベントとコールバックがセットになっているタイプの処理は、event queue に登録される
- call stack > job queue > event queue という優先度で、前者が空になった場合に後者が実行される

https://nodejs.org/en/docs/guides/dont-block-the-event-loop/
https://messagepassing.github.io/012-manycore/01-morrita/
https://nodejs.dev/learn/the-nodejs-event-loop
https://qiita.com/righteous/items/1d6e0396efd43d130a38

### 新たなフレームワークを勉強する時、最初から TypeScript を入れるのは悪手

元々、学習目的で React チュートリアルを TypeScript に置き換えたプログラムを書いており、最初はそれをそのまま乗っけるために TypeScript としてプロジェクトを作成した。その結果、tsc がエラーを吐いたときに、それが自分のコード起因なのか、あるいはライブラリ側の対応が遅れているかなどがわからず、結果開発中はあちこちに any を仕込むハメになった。

今度からは、新規のフレームワークを試すときは、どんな時もまず素の JS でチュートリアルを試し、基本的なことを理解してから TypeScript に移行することを徹底したい。
