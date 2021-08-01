---
title: "Gitでソースコードのバージョンを管理しよう"
---

# Git

## Gitとは

### バージョン管理システムとは

**[Git](https://git-scm.com/)**
とは
**バージョン管理システム(Version Control System (VCS))**
です。

まず、VCSとは一体どういうものなのでしょうか?

そして、なぜVCSが必要なのでしょうか?

バージョン管理とはファイルに加えられる変更を記録してくれるシステムです。

もしあなたが、会社や、学校でExcelファイルを管理したことがあれば、悩ましい問題に直面したはずです。

```txt
A213資料-20210101.xlsx
A213資料-20210102.xlsx
A213資料-20210102v2.xlsx
A213資料(修正版).xlsx
A213資料(修正版)v2.xlsx
A213資料(田中さん修正版).xlsx
```

多くの人々が使っているバージョン管理手法は、ファイル名を変更してコピーするというものです。

このアプローチはとても単純ですが、信じられないほど間違いが起こりやすいものです。

どれを編集しているかわけがわからなくなったり、古いファイルを編集してしまったりしますよね。

VCSを使うことで、ファイルをある時点の状態に戻すことができます。

過去の変更履歴と比較したり、特定の変更を誰が行ったか確認できるようになります。

ここまでで、VCSとは何なのか、バージョン管理がなぜ必要なのかがわかったと思います。

### なぜ、Gitを使うのか

上記で挙げなかった問題がもう一つあります。

他の人と一緒に、同じファイルやファイル群を共同で作業する必要がある場合です。

これを解決するためにいくつかの種類のファイルシステムがあります。

- 集中バージョン管理システム(Centralized Version Control System (CVCS))
  - バージョン管理されたファイルをすべて持つ中央サーバーから作業者はファイルを取ってくるシステム
  - 中央サーバーが使えない時間は、全員が全く作業ができなくなる
  - 中央サーバーが全損した場合、ファイルすべてを失う
- 分散バージョン管理システム(Distributed Version Control System (DVCS))
  - バージョン管理対象のファイル、ディレクトリ、更新履歴などの一群すべてをユーザー間で共有し合う
  - 中央サーバーがないので、あるユーザーの作業によって他のユーザーの作業ができなくなったりしない
  - あるサーバーが故障しても、誰かのバージョン管理対象のファイル、ディレクトリ、更新履歴などの一群すべてをコピーすれば修復できる

Gitはもっとも有名でシェアの高いDVCSです。

そのため、Git以外のVCSは選択肢にまず上がってこないと言って良いでしょう。

DVCSが取り扱う**バージョン管理対象のファイル、ディレクトリ、更新履歴などの一群**を**リポジトリ**といいます。

では実際にGitを使ってバージョン管理しましょう。

### インストール

すでにインストール済みであれば読み飛ばしていただいて結構です。

[Download for macOS](https://git-scm.com/download/mac)

下記のようにGitをインストールし、バージョンを確認してみてください。

```sh
$ brew install git
$ git version
git version 2.31.1 # versionは異なっていても構いません
```

バージョンが表示されれば、インストールは完了しています。

### セットアップ

Gitのセットアップもしておきましょう。

```sh
$ git config --global user.name "taro"
$ git config --global user.email taro@example.com
```

- `"taro"`の部分はあなたが、Gitで使用したい名前を設定してください。
- `taro@example.com`の部分はあなたが、Gitで使用したいメールアドレスを設定してください。

GitはGitHubとの連携を行なう際にGitHubの認証情報が必要になります。毎回入力を求められるのは面倒なのでコンピュータ上にキャッシュできるように設定します。

キャッシュとは一度利用したデータなどを、もう一度使用できるように保存しておく技術をいいます。

```sh
$ git config --global credential.helper osxkeychain
```

これで認証情報が必要になった際にパスワードを求められますが、一度きりになります。

パスワードを求められることを忘れて、びっくりしないようにしてくださいね。

## リポジトリとは

バージョン管理を始める前に**リポジトリ**という用語を復習しておきましょう。

リポジトリとは**バージョン管理対象のファイル、ディレクトリ、更新履歴などの一群**を指します。

Gitにおいてバージョン管理を始める方法は2つあります。

1. あなたのコンピュータ内の特定のディレクトリを、リポジトリとして初期化(リポジトリの作成を)する
2. あなたのコンピュータ内に、あなた以外のコンピュータのリポジトリをコピーする

**あなたのコンピュータ**のことを**local**、**あなた以外のコンピュータ**を**remote**といいます。

それぞれ、localリポジトリ、remoteリポジトリのように表現します。今後頻繁に使用するので覚えておきましょう。

それでは、あなたのコンピュータ内に、リポジトリを作成(つまりlocalリポジトリを作成)しましょう。

## Git init

まず、前回のターミナルの章で作成した`~/code/backenduniv`に
`git-training`というディレクトリを作成し、その中に移動します。

```sh
$ cd ~/code/backenduniv
$ mkdir git-training
$ cd git-training
$ pwd
/Users/taro/code/backenduniv/git-training
```

```sh
~
└── code
      └── backenduniv
            └── git-training
```

それでは`~/code/backenduniv/git-training`をlocalリポジトリとして初期化します。

`git init`を実行しましょう。

```sh
$ git init
Initialized empty Git repository in /Users/taro/code/backenduniv/git-training/.git/
```

`git init`はディレクトリの中に`.git`というディレクトリを作成します。

確認してみましょう。

```sh
$ ls -a
.       ..      .git
```

`.git`は`git init`で自動で生成したディレクトリで、変更履歴などが特殊な表現によって、格納されるようになります。

`git`コマンドはこのディレクトリ内のファイルを読み書きすることでバージョン管理を可能にします。

基本的には`.git`には手を加えないようにしましょう。

この状態ではまだディレクトリの中はファイルは管理対象となっていません。バージョン管理をする準備をしたという状態です。

バージョン管理の対象にできるのは現在のディレクトリ配下のすべてのファイルです。

では、いくつかのファイルを管理の対象としてみましょう。

## Git status

まず、バージョン管理をするファイルがないので作成しましょう。

```sh
$ touch example.txt
$ ls
example.txt
```

どのファイルがどのような状態であるか確認できるコマンド`git status`を使ってみましょう。

```sh
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        example.txt

nothing added to commit but untracked files present (use "git add" to track)
```

> Untracked files:

にexample.txtがあるため、このファイルが追跡されていない(つまりバージョン管理の対象ではない)ことがわかります。

このexample.txtを追跡対象に含めましょう。

## Git add

`git add`コマンドで追跡対象に含めることができます。

```sh
$ git add example.txt
```

もう一度`git status`で状態を確認してみましょう。

```sh
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   example.txt
```

Untracked filesではなくなったのがわかります。

> Changes to be committed:

にexample.txtがあるため、example.txtの変更が次回のコミットに含まれるようになりました。

この次回のコミットに含まれる候補となる変更のことを**インデックス**といいます。

ではこのコミットとはなんでしょうか?

**コミット**とは**ある時点でのリポジトリ全体の状態を保存したもの**です。

**ある時点での全体の状態を保存したもの**はITの分野では**スナップショット**といいます。

つまり、**コミット**とは**スナップショット**だといえます。

ではコミットを作成してみましょう。

## Git commit

`git commit`コマンドでコミットを作成できます。

`-m`コマンドラインフラグはコミットにメッセージをつけるフラグです。

```sh
$ git commit -m "first commit"
[main (root-commit) 78ccc18] first commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 example.txt
```

これではじめてのコミットを作成できました！

`git status`で状態を確認してみましょう。

```sh
$ git status
On branch main
nothing to commit, working tree clean
```

example.txtはコミットに含まれたため、何も表示されないことがわかります。

## Git log

コミット時には**コミットハッシュ**というものがユニークな文字列が採番され、簡単に確認できます。

```sh
$ git log
commit 78ccc18cefc5198d4219519bfddbe0669496b2d0 (HEAD -> main)
Author: taro <taro@example.com>
Date:   Thu May 6 16:39:46 2021 +0900

    first commit
```

> commit 78ccc18cefc5198d4219519bfddbe0669496b2d0

の半角英数字がコミットハッシュです。

## ブランチ

Gitのブランチを理解するためには複数の新しい知識が必要になります。丁寧に手を動かしながら理解していきましょう。

ブランチ(木の枝)が由来しているというのを頭の片隅に置いてください。

まず、ファイルに変更を加えて、状態を見てみます。

```sh
$ echo 'example' > example.txt
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   example.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

> Changes not staged for commit:

にexample.txtが含まれています。

つまり、exmaple.txtの変更は次回のコミットには含まれないということを示しています。

コミットに含めたいので`git add`します。

```sh
$ git add .
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   example.txt
```

`git add .`の`.`は現在のディレクトリを指すので現在ディレクトリ配下のすべてのファイルが`git add`の対象となります。

> Changes to be committed:

にexample.txtが含まれていますので、次回のコミットに含まれるようになったことがわかります。

`git commit`で2回目のコミットを作りましょう。

```sh
git commit -m "second commit"
[main 1644ba2] second commit
 1 file changed, 1 insertion(+)
```

では、`git log`でコミットの状態を確認してみましょう。

```sh
$ git log
commit 1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b (HEAD -> main)
Author: taro <taro@example.com>
Date:   Thu May 6 17:05:19 2021 +0900

    second commit

commit 78ccc18cefc5198d4219519bfddbe0669496b2d0
Author: taro <taro@example.com>
Date:   Thu May 6 16:39:46 2021 +0900

    first commit
```

上記のように2つ目のコミットが作られていることがわかります。

復習ですが、コミットと一対一の`1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b`のような半角英数字をコミットハッシュといいます。

さきほどから無視していた、`HEAD`と`main`について説明します。

これらが、`ブランチ`とよばれ、**あるコミットハッシュに名前をつけたもの**です。

(厳密には[`参照`](https://ja.wikipedia.org/wiki/%E5%8F%82%E7%85%A7_(%E6%83%85%E5%A0%B1%E5%B7%A5%E5%AD%A6))という考え方が絡んできます。気になった場合は見てみてください。)

`main`に`HEAD`という名前をつけて、`1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b`(2回目のコミットのコミットハッシュ)に`main`という名前をつけています。

`HEAD`ブランチはユーザーが現在操作しているブランチを指します。

そのため、コミットしたり、ブランチを切り替えるたびに`HEAD`は動いていきます。

現在は`HEAD`ブランチは`main`ブランチを指しているため、あなたは`main`ブランチで操作しているということになります。

```txt
                HEADブランチ
                   |
                mainブランチ
                   |
最初のコミット -- 2回目のコミット
```

ここで`example`というブランチを作成してみましょう。`git branch`で作成できます。

```sh
$ git branch example
```

`git log`でコミットを確認してみましょう。

```sh
$ git log
commit 1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b (HEAD -> main, example)
Author: taro <taro@example.com>
Date:   Thu May 6 17:05:19 2021 +0900

    second commit

commit 78ccc18cefc5198d4219519bfddbe0669496b2d0
Author: taro <taro@example.com>
Date:   Thu May 6 16:39:46 2021 +0900

    first commit
```

`example`というブランチが`1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b`の別名として作成されていることがわかります。

`git branch`コマンドはブランチ名を入れないとブランチの一覧を表示します。

```sh
$ git branch
  example
* main
```

`*`は`HEAD`ブランチが`main`ブランチであることを示しています。

`HEAD`ブランチを`main`ブランチから、`example`ブランチに変更してみましょう。

```sh
$ git switch example

Switched to branch 'example'
```

```sh
$ git branch
* example
  main
```

`example`ブランチに現在のブランチ(`HEAD`ブランチ)が変更されていることがわかります。

ちなみに`git switch -c`でブランチの作成と移動を同時にできます。

```sh
$ git switch -c example2

Switched to a new branch 'example2'
$ git branch
  example
* example2
  main
```

ではブランチがなんのためにあるのかを理解していきます。

`example`ブランチに戻って、新しいファイルをコミットします。

1. `example`ブランチに`git switch`
2. `example2.txt`を作成する
3. `git add`で`example2.txt`を追跡対象にする
4. `git commit`でコミットを作成する

```sh
$ git switch example
Switched to branch 'example'
$ ls
example.txt
$ touch example2.txt
$ ls
example.txt     example2.txt
$ git status
On branch example
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        example2.txt

nothing added to commit but untracked files present (use "git add" to track)
$ git add example2.txt
$ git status
On branch example
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   example2.txt

$ git commit -m "third commit"
[example fee6cd5] third commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 example2.txt
```

ここで`git log`を確認してみましょう。

```sh
$ git log
commit fee6cd530a6fbb1f8bc34504ccde2f73ea385754 (HEAD -> example)
Author: taro <taro@example.com>
Date:   Thu May 6 20:13:00 2021 +0900

    third commit

commit 1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b (main, example2)
Author: taro <taro@example.com>
Date:   Thu May 6 17:05:19 2021 +0900

    second commit

commit 78ccc18cefc5198d4219519bfddbe0669496b2d0
Author: taro <taro@example.com>
Date:   Thu May 6 16:39:46 2021 +0900

    first commit
```

```sh
commit fee6cd530a6fbb1f8bc34504ccde2f73ea385754 (HEAD -> example)
```

`HEAD`ブランチはユーザーが現在操作しているブランチなのでさきほど`switch`した`example`ブランチを指しています。

その下を見てください！

```sh
commit 1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b (main, example2)
```

もともと`main`ブランチと同じコミットから作成した`example`ブランチですが、
`main`ブランチはもともとのコミット(`second commit`)のままです。

つまり、`main`ブランチには`example2.txt`を追加した`third commit`は反映されていないということになります。

`main`ブランチに移動して確認してみましょう。

```sh
$ git switch main
Switched to branch 'main'
$ git log
commit 1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b (HEAD -> main, example2)
Author: taro <taro@example.com>
Date:   Thu May 6 17:05:19 2021 +0900

    second commit

commit 78ccc18cefc5198d4219519bfddbe0669496b2d0
Author: taro <taro@example.com>
Date:   Thu May 6 16:39:46 2021 +0900

    first commit
```

やはり`second commit`は反映されていません。

```txt
                              HEADブランチ
                                  |
                mainブランチ   exampleブランチ
                   |              |
                   |              |
                   |          3回目のコミット
                   |         /
最初のコミット -- 2回目のコミット
```

つまり、ブランチの役割とは、あるブランチから別のブランチを作成し、
もとのブランチには変更を加えずにコミットを作成するためにあります。

ではもとのブランチに変更を取り込むにはどうすれば良いでしょうか?

## Git merge

まず、`example`ブランチで作成した`example2.txt`が`main`ブランチに存在しないことを確認しましょう。

```sh
# ブランチがmainであることを確認
$ git branch
  example
  example2
* main
$ ls
example.txt
```

`example2.txt`が存在しないことが確認できました。

`example`ブランチで作成したコミットを`main`ブランチに取り込んでみましょう。

`git merge`というコマンドを使います。

```sh
$ git merge --no-f example -m "merge example branch"
Merge made by the 'recursive' strategy.
 example2.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 example2.txt
```

あらためて`git log`を確認しましょう。

```sh
$ git log
commit 890452c3e01e5971fbbbbb6db3531f0872a83ac9 (HEAD -> main)
Merge: 1644ba2 fee6cd5
Author: taro <taro@example.com>
Date:   Thu May 6 20:38:55 2021 +0900

    merge example branch commit

commit fee6cd530a6fbb1f8bc34504ccde2f73ea385754 (example)
Author: taro <taro@example.com>
Date:   Thu May 6 20:13:00 2021 +0900

    third commit

commit 1644ba22b5ffd5bdc658aaccdd8eb6457e9d447b (example2)
Author: taro <taro@example.com>
Date:   Thu May 6 17:05:19 2021 +0900

    second commit

commit 78ccc18cefc5198d4219519bfddbe0669496b2d0
Author: taro <taro@example.com>
Date:   Thu May 6 16:39:46 2021 +0900

    first commit
```

```sh
commit 890452c3e01e5971fbbbbb6db3531f0872a83ac9 (HEAD -> main)
```

`git merge` によって新しいコミットが作られました。(`merge example branch commit`)

このコミット(`merge example branch commit`)の下に表示されているのは`example`ブランチで作成した`thrid commit`です。

```txt
                                              HEADブランチ
                                                   |
                              exampleブランチ  mainブランチ
                                  |                |
                                  |                |
                              3回目のコミット　　     |
                             /             \       |
最初のコミット -- 2回目のコミット                 マージコミット
```

つまり、`git merge`下記のような流れになっています。

1. `example`ブランチのコミットログを取り込む。
2. `example`ブランチの変更と`main`ブランチの変更を計算し、新しいマージコミットを作成する。


では、3回目のコミットとマージコミットの間に`main`ブランチで新しいコミットを作成していたらどうなるでしょうか?

試してみたいのですが、もうマージしてしまったので状態を戻す必要があります。

## Git reset

`git reset`はコミットやインデックスの状態を元に戻すためのコマンドです。

かなりややこしいコマンドです。

ここでは最新のコミットを消して一つ前のコミットに`HEADブランチ`を戻す方法だけを教えます。

あらためて、`git log`で`HEAD`ブランチが`main`ブランチにあるかを確認します。

```sh
git log --pretty=oneline --abbrev-commit

890452c (HEAD -> main) merge example branch commit
fee6cd5 (example) third commit
1644ba2 (example2) second commit
78ccc18 first commit
```

`--pretty=oneline`は`git log`を一行にまとめてくれます。

`--abbrev-commit`は短縮したコミットハッシュにして表示してくれます。

下記のコマンドで直前のコミットを消して一つ前のコミットに`HEAD`ブランチを移動させます。

```sh
$ git reset --hard HEAD^
HEAD is now at 1644ba2 second commit
```

`git log`を確認します。

```sh
git log --pretty=oneline --abbrev-commit

1644ba2 (HEAD -> main, example2) second commit
78ccc18 first commit
```

直前のコミットなので、`thrid commit` は残るのではと思ったと思います。

しかし、`third commit`は`example`ブランチで作成したコミットで、実態はマージコミットの中にあります。

`git log`上では`example`ブランチで作成したコミットとして見えているだけだったのです。

なのでマージコミットと一緒に`main`ブランチからは`third commit`もなくなります。

下記の状態に戻りました。

```txt
                              HEADブランチ
                                  |
                mainブランチ   exampleブランチ
                   |              |
                   |              |
                   |          3回目のコミット
                   |         /
最初のコミット -- 2回目のコミット
```

では、あらためて、3回目のコミットとマージコミットの間に`main`ブランチで新しいコミットを作成していたらどうなるか検証してみましょう。

1. `main`ブランチで`example3.txt`を作成し、コミットする
2. `example`ブランチをマージする

```sh
$ touch example3.txt
$ git add example3.txt
$ git commit -m "fourth commit"
[main dc4ecf4] fourth commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 example3.txt
$ git merge example -m "second merge commit"
Merge made by the 'recursive' strategy.
 example2.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 example2.txt
```

`git log`を確認してみましょう。

```sh
$ git log --pretty=oneline --abbrev-commit

03ab007 (HEAD -> main) second merge commit
dc4ecf4 fourth commit
fee6cd5 (example) third commit
1644ba2 (example2) second commit
78ccc18 first commit
```

時系列通りに`example`ブランチで作成した`thrid commit`のあとに、
`main`ブランチで作成した`fourth commit`を行っています。

```txt
                                                          HEADブランチ
                                                                |
                              exampleブランチ              mainブランチ
                                  |                             |
                                  |                             |
                              3回目のコミット　　                  |
                             /             \                    |
最初のコミット -- 2回目のコミット          4回目のコミット  --   マージコミット
```

```sh
$ ls
example.txt     example2.txt    example3.txt
```

`example`ブランチで作成した`example2.txt`も`main`ブランチで作成した`example3.txt`も共に存在しています。

このようにブランチを作成し、マージするまでの間にもとのブランチで変更を加えても問題なくマージができます。

この機能のおかげで、他の人と並列に開発ができるので、Gitはチーム開発に非常にマッチします。

では、他の人のブランチをどのようにマージできるのでしょうか?

## GitHub

localリポジトリ、remoteリポジトリを覚えてますか?

あらためて説明します。

あなたのコンピュータのリポジトリをlocalリポジトリ、
あなた以外のコンピュータのリポジトリをremoteリポジトリといいます。

今まで作業していたリポジトリはlocalリポジトリでした。

チームで開発するときは、remoteリポジトリを用意することになります。

必要に応じてremoteリポジトリにデータの取得、同期をする必要があります。

今回はチーム開発ではないので下記までを行います。

1. remoteリポジトリの作成
2. localリポジトリをremoteリポジトリに同期させる

remoteリポジトリの管理(Gitホスティングといいます)でサービスが[Github](https://github.com/)です。

すでにアカウントは取得済みのはずなので、GitHubにリポジトリを作成しましょう。

### GitHubリポジトリの作成

<https://github.com/new>からリポジトリの作成ができます。

今回はGitの練習のためのリポジトリですので`Repository name`の箇所だけを記入してください。

`Repository name`はlocalリポジトリと一致させるのが望ましいので`git-training`としてください。

`Public`と`Private`というチェックボックスがあると思います。

- Public: 公開リポジトリ
- Private: 非公開リポジトリ

今回は公開されても、なんら問題のないリポジトリなので`Public`を選択しましょう。

今後、リポジトリを何個も作ることになると思いますが、基本的には誰かに知られては困る内容(機密情報)はコミットしないようにしてください。

`Initialize this repository with:`のチェックボックスは選択**しない**でください。

最後に`Create Repository`を押しましょう。作成完了です。

## Git remote

ではローカルのリポジトリでの操作に戻りましょう。

remoteリポジトリとの連携をするためのコマンドが`git remote`です。

```sh
$ git remote -v
```

何も出力されないはずです。

remoteリポジトリを設定していないためです。

設定しましょう。

```sh
git remote add origin https://github.com/taro/git-training.git
```

`git remote add`はremoteリポジトリを設定するコマンドです。

ここで`origin`とはremoteリポジトリの登録名です。`origin`と登録するのが通例となっています。

しかし、どんな名前をつけても構いません。

`https://github.com/taro/git-training.git` の`taro`はあなたのGitHubのユーザー名を代わりに入力してください。

リポジトリ作成後の画面に表示されるのでそれをコピーしても構いません。

![GitHub Repository](https://res.cloudinary.com/dpnkkfkox/image/upload/v1627694180/git-remote-repository_iypm5q.png)

設定を再度確認してみましょう。

```sh
$ git remote -v
origin  ssh://git@github.com/taro/git-training.git (fetch)
origin  ssh://git@github.com/taro/git-training.git (push)
```

下記の2つが設定されます。
- fetch: 読み取り用
- push: 書き込み用

ではremoteリポジトリにlocalのリポジトリを同期させてみましょう。

## Git push

`git push` コマンドを使います。

```sh
$ git push -u origin main
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 12 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (12/12), 980 bytes | 980.00 KiB/s, done.
Total 12 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (3/3), done.
To ssh://github.com/taro/git-training.git
 * [new branch]      main -> main
```

```sh
git push -u {remote名} {ブランチ名}
```

`-u`は`upstream branch`の略で、localのブランチをremoteの、どのブランチと対応しているのかを示すものだと思ってください。

今回のコマンドdはlocalリポジトリの`main`ブランチの`upstream branch`をremoteリポジトリの`main`ブランチにするように指示しています。

ブラウザを更新して、GitHub上に作成したリポジトリを見てみてください！

`example.txt`、`example2.txt`、`example3.txt`が表示されていればOKです！remoteリポジトリにlocalリポジトリの`main`ブランチをpushできました！

このように、remoteリポジトリへの反映はブランチを指定して、行なうことであると覚えておきましょう。

続いて、remoteブランチの`main`ブランチをlocalの`main`ブランチにマージする方法を見てきましょう。

## GitHubの操作

1. ブラウザでGitHubの先ほど作成したリポジトリを開きます
2. `Add file`を押してください
3. `Create new file`を押してください
4. `example4.txt`という名前でファイルを作成します

  ![GitHub Add file](https://res.cloudinary.com/dpnkkfkox/image/upload/v1627694293/git-remote-add-file_hny0hr.png)

5. `fifth commit`というコミットメッセージにします。

  ![GitHub Add file](https://res.cloudinary.com/dpnkkfkox/image/upload/v1627694295/git-remote-add-file-commit-message_lxzagw.png)

6. `Commit directly to the main branch.`のままで`Commit new file`を押します。

remoteの`main`ブランチに新しいコミットが作成できました。

今回はGit操作の練習のために行いましたが、基本的にGitHubから直接ファイルを操作することはないので忘れていただいて大丈夫です。

## Git fetch

あらためてlocalリポジトリの`git log`を確認します。

```sh
$ git log --pretty=oneline --abbrev-commit

03ab007 (HEAD -> main, origin/main) second merge commit
dc4ecf4 fourth commit
fee6cd5 (example) third commit
1644ba2 (example2) second commit
78ccc18 first commit
```

ではlocalの`main`ブランチに`remote`の`main`ブランチを取り込むためにはlocalリポジトリにremoteリポジトリの`main`ブランチを持ってくる必要があります。

`git fetch`コマンドで行います。

```sh
$ git fetch origin main
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 629 bytes | 209.00 KiB/s, done.
From ssh://github.com/taro/git-training
 * branch            main       -> FETCH_HEAD
   03ab007..145092f  main       -> origin/main
```

`FETCH_HEAD`ブランチとはremoteの`main`ブランチのコミットハッシュの別名です。

fetchした当時の最新のコミットハッシュです。

`origin/main`とはremoteとして設定した`origin`の`main`ブランチです。

これで、最新のコミットを取り込んだremoteの`main`ブランチがlocalリポジトリに存在する状態になりました。

`git branch -a`で`fetch`済みであれば、`remote`のブランチを見ることができます。

`-a`はAllの略です。

```sh
git branch -a
  example
  example2
* main
  remotes/origin/main
```

では`remote`の`main`ブランチ(`origin/main`ブランチ)を`local`の`main`ブランチに取り込んでみましょう。

```sh
$ git merge origin/main -m "merge origin main branch commit"
Merge made by the 'recursive' strategy.
 example4.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 example4.txt
```

```sh
$ git log --pretty=oneline --abbrev-commit

ef6fc65 (HEAD -> main) merge origin main branch commit
145092f (origin/main) fifth commit
03ab007 second merge commit
dc4ecf4 fourth commit
fee6cd5 (example) third commit
1644ba2 (example2) second commit
78ccc18 first commit
```

取り込めていることを確認できました。

## Git pull

`git fetch` => `git merge`は一緒にやることがほとんどです。

なのでこの一連のコマンドが合体したコマンドがあります。

`git pull`コマンドです。

```sh
$ git pull origin main
```

先ほどのようにブランザからGitHubのリポジトリに変更を加えてみてから、実際にやってみてください。

## おわりに

これで必要最低限のGitの学習は終わりです。次の学習に進んできましょう！

しかし、ここで紹介した知識だけでは、チーム開発には全く足りていません。

大丈夫です。まずは、ここで紹介した知識をしっかり身につけて、Gitに慣れましょう。

次回は、環境構築が簡単にできるようになる**Docker**について学んでいきましょう。
