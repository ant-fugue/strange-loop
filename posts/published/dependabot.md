---
title: "Dependabotに警告されてnpmパッケージを更新した"
created: "2021-10-10"
updated: "2021-10-10"
---

前から[このレポジトリ](https://github.com/searchingforapuppy/oeis)に対して、Dependabot 経由で security alert が度々送られてきており、今日ようやくアップデートした。

## 概要

Dependabot は、脆弱性のある依存関係を検知して、プルリクを作成し、脆弱性の修正を含む最小バージョンに自動でアップグレードしてくれる。しかし、さまざまな要因によってプルリクの発行がブロックされることがあり、その場合はユーザに警告が飛ぶので、それを読んで自分でどう対処するか考えなければならない。(エラーの分類と調査については[こちら](https://docs.github.com/ja/code-security/supply-chain-security/managing-vulnerabilities-in-your-projects-dependencies/troubleshooting-dependabot-errors))

今回ひっかかっていたのは、[path-parse](https://github.com/jbgutierrez/path-parse)と[tmpl](https://www.npmjs.com/package/tmpl)の二つのパッケージ。正規表現の性能等に関してのアラートだった。

面白かったのは、両者ともに npm でのインストール数は週間数千万をたたき出す大人気パッケージなのに、GitHub のスター数を見ると、前者は 100 程度、後者に至っては 10 しかない点である。コード自体も、テスト含めても前者で数百行、後者は数十行のちっぽけなパッケージである。

なぜこういうことが起こるかと言うと、前者は node の path.parse()を拡張したモジュールで、これが同期・非同期なファイル操作に使える node の require.resolve()代用モジュールに使われたかららしい。後者は、作者自らがこのパッケージを組み込んだディレクトリ探索パッケージ (walker という) を publish しており、それが jest で使われているから、ということだった。

## 修正

気の迷いで、はじめは package-lock.json を直接触ってバージョンをあげてみたが、当然テストに落ち、npm update => npm audit fix で修正して CI/CD を通し、事無きを得た。
