---
title: "sqlite tutorialでSQL入門"
created: "2021-10-26"
updated: "2021-10-26"
---

## 概要

[sqlite 公式チュートリアル](https://www.sqlitetutorial.net/)で SQL に入門する

[sqlite チュートリアルのサンプルデータベース](https://www.sqlitetutorial.net/sqlite-sample-database/)

### SELECT

SELECT より先に FROM が評価される
FROM で指定するのはテーブル、SELECT で指定するのはテーブルのコラム

```SQL
SELECT
  trackid,
  name,
  composer,
  unitprice
FROM
  tracks;
```
