---
title: "Underlying technology of Docker: namespaces"
created: "2021-11-26"
updated: "2021-11-26"
---

趣味でも仕事でも、Docker を使う機会は非常に多いけれども、いくら Dockerfile をコピペ・編集してコンテナを立ち上げたところで、その裏側で何が起きているのかはよくわからない。そもそも、Docker 自体が会社名、CLI ツール、mac/win 向けアプリ、イメージレジストリなど、多くのものを指すのが、混乱する。[イラストでわかりやすく解説してくれているスライド](https://www.slideshare.net/KoheiTokunaga/ss-122754942)もあったが、どうもイメージが湧かず、困っていたところ、ついに[素晴らしい動画](https://www.youtube.com/watch?v=-YnMr1lj4Z8)を見つけた。

この動画を参考に、Docker が結局のところ何をしているかをまとめると、こういうことだろうか。

1. docker CLI が発行するコマンドを dockerd(docker demon)が受け取る
2. dockerd は containerd を呼び出す
3. containerd は runc を呼び出す
4. runc は親プロセスから子プロセスを呼び出して clone した後で、子プロセスの存在する namespace に異なるプロセス ID とユーザ ID を付与して隔離し、親プロセスにアクセスできなくする一連の systemcall を呼び出す
