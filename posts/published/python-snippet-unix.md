---
title: "Pythonでetc/passwdからユーザ名取得"
date: "2021-09-19"
---

ちょっとした空き時間での新しい言語の学習用に、コードスニペットを活用することにした。
公式ドキュメントや、スニペットサイトを見て、適当に選んだものを貼り、それの挙動を調べる。

今回はこれ。

```python
print('\n'.join(line.split(":",1)[0] for line in open("/etc/passwd")))
```

UNIX 系の OS で、パスワードファイル上の全てのユーザを出力する。
([このサイト](https://pythonsnippets.dev/snippet/60/)から引用)。

## [join()](https://docs.python.org/3/library/stdtypes.html?highlight=join#str.join)

- Iterable を引数に取り、文字列を返す
- join の前の"."は、Iterable の要素の区切りをどうやるか指定する
- 下の例では例スペースを追加する。"\n"だったら改行を挿入する

```python
words = ["How","are","you","doing","?"]
sentence = ' '.join(words)
print(sentence)
```

## [open()](https://docs.python.org/3/library/functions.html#open)

- ファイルを引数に取り、ファイルオブジェクトを返す
- ファイルが開けなかったら、エラーを返す
- 第二引数には読み取り専用、書き込み専用などの mode を指定する文字が入る
- デフォルトでは読み取り専用

## [str.split()](https://docs.python.org/3/library/stdtypes.html?highlight=split#str.split)

- 文字列を引数に取り、文字列のリストを返す
- 第一引数は各要素の区切り文字
- 第二引数は split の上限を設定

```python
>>> '1,2,3'.split(',')
['1', '2', '3']
>>> '1,2,3'.split(',', maxsplit=1)
['1', '2,3']
>>> '1,2,,3,'.split(',')
['1', '2', '',**** '3', '']
```

以上を踏まえてこのスクリプトがやっているのは、

1. /etc/passwd からファイルを読み込む
2. ファイルオブジェクトからユーザ名のリストを取り出す
3. リストの要素ごとに改行を挿入する
4. コンソールに出力する

である。
