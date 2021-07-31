---
title: "DockerでRuby on Railsの環境構築をしよう"
---

# Docker

本チャプターの目的はDockerというものを使ってRuby on Railsの環境構築をすることです。

Dockerの説明には、メモリ、CPU、その他技術的な用語が多く含まれます。

その点はあまり気にせず、Dockerの使い方とRuby on Railsの環境構築ができるようになることのみに注力するようにしてください。

## Docker とは

Dockerを理解するには複数の専門用語を要します。

そのため、まずは専門用語を廃した平易な文章で説明します。

その後に、専門用語を交えてより込み入った説明をします。

## Dockerは何のためにあるのか

一言でいうと、OSの中にOSを作り出すツールです。

例えば、Dockerを使うと、
設定ファイルを用意すれば、あなたのmacOS内に、LinuxOSを作り出だすことができます。

LinuxOSとは無料で使用できるmacOSとは別のOSです。

しかも、同じ設定ファイルを使えば、完全に同一な設定を持ったLinuxOSを別のコンピュータのOSにも作り出すことができます。

これにはいくつかのメリットがあります。

- 手元のコンピュータでは動くのに、サーバー上で動かずに障害が発生するということがなくなる
- 開発者のコンピュータに環境を簡単に用意できる
- 複数のコンピュータに同一のコードを実行できるサーバーを簡単に用意できる
- サーバーへのアクセスが増加したときに新しいサーバーを簡単に用意できる
- サーバーがダウンしたときにすぐに別のサーバーを立ち上げられる

この講座では、コードの実行環境をすべてDockerで準備します。
- 開発者のコンピュータに環境を簡単に用意できる
という恩恵が受けられるためです。

Dockerは初心者がつまずきやすい、環境構築を非常に簡単にしてくれます。

## 仮想環境

OSの中にOSを作るというのが**仮想環境**というものです。

仮想という表現から分かるとおり、実際にはCPUや、メモリは共有し合います。

論理的にCPUやメモリを割り当てて、あたかも別のOSが存在するかのようにしてくれます。

ユーザーは仮想環境であることを意識することなく、独立したOSとして仮想環境を扱うことができます。

## 最低限必要な3つの用語

仕組みを完全に理解することはここでは不可能だと思います。

なので、3つの用語をまず覚えましょう。そうすればDockerを使用できます。

- Dockerコンテナ
- Dockerイメージ
- Dockerfile

DockerではOS上に**Dockerコンテナ**という単位の実行環境(仮想環境)を構築します。

仮にmacOS内に、LinuxOSを作りたいとした場合、ここで作られるLinuxOSが**Dockerコンテナ**です。

Dockerコンテナを立ち上げるためには**Dockerイメージ**が必要になります。

Dockerイメージはコンテナを実行するために必要なすべてです。(コード、ファイルシステム、依存関係等)

ここでどんなものなのか具体的に理解する必要はありません。

Dockerイメージの構築するための設定ファイルが**Dockerfile**です。

つまり、Dockerコンテナを立ち上げるためには以下のフローをたどります。

- Dockerfileを記述する
- Dockerfileを元にDockerイメージを作成する
- Dockerイメージを指定してDockerコンテナを実行する

ここで説明した知識では十分ではありません。

下記のドキュメントを参考にしてみてください。

[Docker ドキュメント](https://docs.docker.jp/)

とにかく手を動かして慣れていきましょう。

## インストール

下記からダウンロードしてください。

[Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)

- M1チップ搭載の場合は`Mac with Apple chip`から
- それ以外は`Mac with Intel chip`から

ダウンロードが完了したらSpotlight検索(メニューバーにある虫眼鏡マーク)にDockerと入力して、起動してください。

```sh
$ docker --version
Docker version 20.10.5, build 55c4c88
```

情報が表示されればダウンロードできています！

実践に取り掛かっていきましょう。

## セットアップ

`~/code/backenduniv`に`docker-training`というディレクトリを作成し、その中に移動します。

```sh
$ cd ~/code/backenduniv
$ mkdir docker-training
$ cd docker-training
$ pwd
/Users/taro/code/backenduniv/docker-training
```

前回学んだGitを使っていきましょう。

ディレクトリをリポジトリとして初期化します。

```sh
$ git init
Initialized empty Git repository in /Users/taro/code/backenduniv/docker-training/.git/
```

今後もなるべくGitで細かくコミットをしていく練習をしましょう。

Gitのコミットの粒度や、コミットメッセージ等に悩んだ場合はAppendixを参照してください。

### Dockerを体感する

Dockerをざっくりと体感して、どんなものなのか理解していきましょう。

まず、`docker-training`ディレクトリに`example`ディレクトリを作成して、移動してください。

```sh
$ mkdir example
$ cd example
$ pwd
/Users/taro/code/backenduniv/docker-training/example
```

## Dockerfileの作成

Dockerfileを作成していきましょう。

ファイル名はそのままです。Dockerfileというファイルを作成してください。

```sh
$ touch Dockerfile
```

Dockerfileに`FROM ubuntu:latest`と記載します。

```sh
$ echo 'FROM ubuntu:latest' > Dockerfile
```

Dockerfileは必ず`FROM`命令から始まります。

`FROM`はベースとなるイメージを設定します。

つまりこのDockerfileで作成されるDockerイメージは`ubuntu:latest`イメージをベースとして作成します。

ベースにしたいような主要なOS、プログラミング言語のイメージは公式で提供されている場合がほとんどです。

今回はLinuxOSのひとつであるUbuntuを使用しているということです。

`ubuntu:lastest`の`ubuntu`はDockerリポジトリとよばれるものです。

`ubuntu:lastest`の`latest`はDockerタグとよばれるものです。

Dockerタグはバージョンと捉えてもらって構いません。

今回は最新のUbuntuをベースイメージとするということになります。

## Docker image build

では、作成したDockerfileからDockerイメージを作成しましょう。

`docker image build`コマンドを使います。

- `.` で`Dockerfile`があるパスを指定します
- `-t`でイメージに`リポジトリ:タグ`を付けます

```sh
$ docker image build . -t example:latest
[+] Building 11.1s (6/6) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 61B
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [internal] load metadata for docker.io/library/ubuntu:latest
 => [auth] library/ubuntu:pull token for registry-1.docker.io
 => [1/1] FROM docker.io/library/ubuntu:latest@sha256:cf31af331f38d1d7158470e095b132acd126a7180a54f263d386da88eb681d93
 => => resolve docker.io/library/ubuntu:latest@sha256:cf31af331f38d1d7158470e095b132acd126a7180a54f263d386da88eb681d93
 => => sha256:cf31af331f38d1d7158470e095b132acd126a7180a54f263d386da88eb681d93 1.20kB / 1.20kB
 => => sha256:86ac87f73641c920fb42cc9612d4fb57b5626b56ea2a19b894d0673fd5b4f2e9 943B / 943B
 => => sha256:7e0aa2d69a153215c790488ed1fcec162015e973e49962d438e18249d16fa9bd 3.31kB / 3.31kB
 => => sha256:345e3491a907bb7c6f1bdddcf4a94284b8b6ddd77eb7d93f09432b17b20f2bbe 28.54MB / 28.54MB
 => => sha256:57671312ef6fdbecf340e5fed0fb0863350cd806c92b1fdd7978adbd02afc5c3 851B / 851B
 => => sha256:5e9250ddb7d0fa6d13302c7c3e6a0aa40390e42424caed1e5289077ee4054709 187B / 187B
 => => extracting sha256:345e3491a907bb7c6f1bdddcf4a94284b8b6ddd77eb7d93f09432b17b20f2bbe
 => => extracting sha256:57671312ef6fdbecf340e5fed0fb0863350cd806c92b1fdd7978adbd02afc5c3
 => => extracting sha256:5e9250ddb7d0fa6d13302c7c3e6a0aa40390e42424caed1e5289077ee4054709
 => exporting to image
 => => exporting layers
 => => writing image sha256:f2bed4af2163116084464018b01076094f252d4c7fee7bad02f8e73ef7a4e78d
 => => naming to docker.io/library/example:latest
```

Dockerイメージを作成できました。

Dockerイメージは現在のディレクトリに作成されるわけではありません。

```sh
$ ls -a
.		..		Dockerfile # 何も作られていない
```

そして、それはどこであるかわたしたちは認識する必要すらありません。すべてDockerが管理してくれます。

`docker image ls`で確認してください。

```sh
$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
example      latest    f2bed4af2163   13 days ago   72.7MB
```

Dockerイメージが作成されていることを確認できました。

## Docker image pull

今回作成したイメージのベースイメージ`ubuntu:latest`は事前にダウンロードしておくこともできます。

`docker image pull`でできます。

```sh
$ docker image pull ubuntu:latest
latest: Pulling from library/ubuntu
345e3491a907: Already exists
57671312ef6f: Already exists
5e9250ddb7d0: Already exists
Digest: sha256:cf31af331f38d1d7158470e095b132acd126a7180a54f263d386da88eb681d93
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

`Already exists`となっています。

先ほどのビルドでダウンロード済みだったためです。

つまり、Dockerfileの`FROM ~`は`docker image pull`を実行しているのとほぼ同じということです。

## 一応覚えておいてほしいこと

回線が安定しない環境で`docker image pull`や`docker build`を行なうとDockerイメージのダウンロード時に途中で止まる可能性があります。

そうなってしまった場合は、メニューバーにある`Docker Desktop`アプリケーション(クジラにブロックが乗っているようなアイコン)を開きます。

![Docker Icon](https://res.cloudinary.com/dpnkkfkox/image/upload/v1627698700/docker-icon_g2i8ze.png)

`Preferences...`を押してください。

![Docker Task Bar](https://storage.googleapis.com/zenn-user-upload/f55db320c597f96145affc86.png)
<img src="./../public/docker/task-bar.png" width="320" alt="Docker Task Bar">

`Docker Engine` を選択してください。

<img src="./../public/docker/docker-engine.png" alt="Docker Engine">

テキストフィールドに下記に変更してください。

```sh
{
  "debug": true,
  "experimental": false,
  "max-concurrent-downloads": 1
}
```

`Apply & Restart`を押してDockerを再起動します。

これでダウンロードが途中で止まらなくなるはずです。

## Docker container run

では、Dockerイメージを指定して、Dockerコンテナを実行して、LinuxOSを操作しましょう。

`docker container run`コマンドで実行できます。

```sh
$ docker container run example:latest
$
```

何も起きませんでしたか?

では`docker container ls`でコンテナがあるか確認してみましょう。

```sh
$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

やはり何もありません。

`ls`コマンドと同じで`-a`フラグが`docker container ls`にもあります。

```sh
$ docker container ls -a
CONTAINER ID   IMAGE            COMMAND       CREATED          STATUS                      PORTS     NAMES
405b5de9f51f   example:latest   "/bin/bash"   54 seconds ago   Exited (0) 53 seconds ago             epic_brown
```

ありました！しかし、`STATUS`に`Exited`とあります。

つまり、実行はしたが、終了した、ということがわかりました。

## コンテナの実行目的

これはコンテナというものが、OSを実行することが目的ではないことに由来します。

もちろんOSも実行できますが、**プロセス**を実行することが目的だからです。

プロセスとは何かの処理と捉えてください。プログラムや、計算処理などのあらゆるものが含まれます。

なのでコンテナはプロセスが終了すると、一緒に終了します。

今回はDockerfileで何もプロセスを指定していませんので、終了してしまった、ということになります。

今回はLinuxOSを操作するのが目的なので、それでは困ります。

もう少し出力結果を見てみましょう。

## /bin/bash

COMMANDに`"/bin/bash"`とあります。これはなんでしょうか?

`/bin/bash`とはUbuntu上のパスです。`/bin`はバイナリデータの略です。

バイナリデータとは2進数表現されるデータです。コンピュータは2進数表現を直接処理できます。

つまり、コンピュータが直接実行するファイルが`/bin`には格納されています。

ターミナルでいつも実行している`ls`や`pwd`も`/bin`に格納されています。

では、`/bin`にある`bash`とは一体何なのかというとシェルの一種です。

ターミナルの章で少し触れましたが、ターミナル内で表示しているソフトウェア(黒い画面)をシェルといいます。

皆さんは`Zsh`を使用してますが、Ubuntuのシェルは`Bash`なので`/bin/bash`を呼び出しているということになります。

## Docker container run -it

`/bin/bash`がシェルで、その実行が終了したためにコンテナも終了したということがわかりました。

つまりシェルが終了しないようにすればよいということになります。

シェルは2つのものを必ず必要とします。

1. ターミナル
2. 標準入力

**標準入力**(Standard input)とはキーボードからの入力です。

キーボードではないファイル等に入力もとを切り替えることで変更できます。

そのため、シェルを起動したときには一緒に、ターミナルと標準入力を割り当ててあげる必要があります。

`docker container run`に`-it`フラグを付与するこでできます。

`-it`は`-i`と`-t`の合成です。

- `-i`はSTDIN(standard input)の略
- `-t`はTTY(teletypewriter)の略

実行してみましょう。

```sh
$ docker container run -it example:latest
root@6b5d79e0317f:/#
```

Ubuntuのコンテナを実行し、その中のシェルを起動できました！

シェルは入力を受け付けています。Ubuntuであることを確認してみましょう。

(Ubuntuのbashシェルは`#`以降が標準入力です。)

```sh
root@6b5d79e0317f:/# cat /etc/issue
Ubuntu 20.04.2 LTS \n \l
```

Ubuntuであることを確認できました！

では、今どのディレクトリにいて、どんなものがディレクトリに入っているのか確認してみましょう。

```sh
root@6b5d79e0317f:/# pwd
/
root@6b5d79e0317f:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

コンテナの中のOSとコンテナの外のOSは完全に分離しているのでコンテナの外のファイルはコンテナの中にはありません。

ここまでで一通りの使い方は学べました。

それではDockerについてより深く学んできましょう。

下記でコンテナの外に出れます。

```sh
root@6b5d79e0317f:/# exit
exit
$
```

## Dockerで他に知っておく必要があること

### Dockerコンテナにディレクトリをマウントする

コンテナの中のOSとコンテナの外のOSは完全に分離しているといいましたが、<br>
ファイルを持っていくことができなければ、プログラムを実行できません。

ここでは、ディレクトリをコンテナ内にマウントすることでファイルを持っていく方法を学びましょう。

**マウント**とは**ファイルやディレクトリを共有すること**です。

ではやってみましょう。

実行するコマンドには新しい知識がいっぱい含まれるので一つ一つ説明していきます。

```sh
$ echo $PWD
/Users/taro/code/backenduniv/docker-training/example
```

`$PWD`とは**環境変数**というものです。

環境変数とはOSがよく使う値を自由に使えるように準備してくれている便利機能だと思ってください。

`$PWD`環境変数は現在のディレクトリのパスが格納されていて、`pwd`コマンドの値と同じです。

では`$PWD`と合わせてコンテナを実行します。

```sh
$ docker container run  -v $PWD:/example -w /example -it example:latest
root@3cab4ab54ada:/example#
```

- `-v` Volumeの略
  - `-v {コンテナの外のパス}:{コンテナの外のパス}`とすることでディレクトリをマウントできる
  - 絶対パスでないといけない
- `-w` Working Directoryの略 `-w {コンテナの外のパス}`とすることでそのパスでコンテナが実行される
  - 絶対パスでないといけない

`$PWD`は`docker container run -v`だけでなく、コマンドの中で現在のディレクトリを使用したいときに合わせて使われる事が多いです。

つまり`docker container run  -v $PWD:/example -w /example -it example:latest`というコマンドは下記のような意味になります。
- 現在のディレクトリ(コンテナの外)と`/example`ディレクトリ(コンテナの中)をマウントし(`-v`)
- `/example`ディレクトリ(コンテナの中)で実行する(`-w`)

コンテナの中で確認してみましょう。

```sh
root@28384bbfe2a2:/example# pwd
/example
root@28384bbfe2a2:/example# ls
Dockerfile
root@28384bbfe2a2:/example#
```

コンテナの中にDockerfileが見つかりました！

コンテナの中のディレクトリが`/example`となり、コンテナの外のディレクトリとファイルが共有されていることが確認できました。

コンテナから出ておきましょう。

```sh
root@28384bbfe2a2:/# exit
exit
$
```

次回から別のDockerfileを作成していくのでここで一度コミットしておきましょう。

```sh
$ cd ../
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        example/

nothing added to commit but untracked files present (use "git add" to track)
$ git add example
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   example/Dockerfile

$ git commit -m "add example Dockerfile"
[main (root-commit) 5703c05] add example Dockerfile
 1 file changed, 1 insertion(+)
 create mode 100644 example/Dockerfile
```

### Dockerfile2

より実践的なDockerfileについて説明していきます。

Ruby on Railsアプリケーションを作成するのに必要最小限の設定をDockerfileとして用意します。

特にRubyやRuby on Railsの知識はいらないので安心してください。

まず、`~/code/backenduniv/docker-training`に`rails-example`というディレクトリを作成しましょう。

```sh
$ mkdir ~/code/backenduniv/docker-training/rails-example
```

`rails-example`ディレクトリにDockerfileと`Gemfile`、`Gemfile.lock`を作成しましょう。

```sh
$ touch rails-example/Dockerfile
$ touch rails-example/Gemfile
$ touch rails-example/Gemfile.lock
```

`rails-example/Dockerfile`を下記のように修正してください。

```docker
FROM ruby:3.0

ENV LANG=C.UTF-8 \
    TZ=Asia/Tokyo

WORKDIR /app

COPY Gemfile /app/Gemfile

COPY Gemfile.lock /app/Gemfile.lock

RUN bundle install

EXPOSE 3000

CMD rails server -b 0.0.0.0
```

`rails-example/Gemfile`を下記のように修正してください。

```
source 'https://rubygems.org'
gem 'rails', '6.1.3.2'
```


下記のような状態になりました。

```sh
docker-training # 今いるディレクトリ
  └── rails-example
        ├── Dockerfile
        ├── Gemfile
        └── Gemfile.lock # 空のファイル
```

`rails-example/Dockerfile`でDockerfileについて説明していきます。

```docker
FROM ruby:3.0                       # ruby:3.0をベースイメージとして使用する

ENV TZ=Asia/Tokyo                   # 環境変数TZにAsia/Tokyoを設定

WORKDIR /app                        # 作業ディレクトリを/appに設定(コンテナ内)

COPY Gemfile /app/Gemfile           # コンテナ外のGemfileをコンテナ内の/app/Gemfileにコピーする

COPY Gemfile.lock /app/Gemfile.lock # コンテナ外のGemfile.lockをコンテナ内の/app/Gemfile.lockにコピーする

RUN bundle install                  # bundle install コマンドを実行

EXPOSE 3000                         # 3000番ポートをコンテナ外に公開する

CMD rails server -b 0.0.0.0         # rails server -b 0.0.0.0 コマンドを実行
```

#### FROM

`FROM ruby:3.0`は前回説明したとおり、ベースイメージを設定するものです。

`Dockerfile`内で大英字になっているものは**命令**とよばれ、今回は`FROM`命令とよびます。

#### ENV

`ENV`命令は環境変数を設定します。

コンテナの中で`echo TZ`とすれば`Asia/Tokyo`と標準されます。

#### WORKDIR

`WORKDIR`命令はワーキングディレクトリを設定します。

コンテナ内のどのディレクトリで実行が開始するかを指定します。

#### COPY

コンテナの外から中にファイルをコピーする命令です。

#### RUN

指定されたコマンドを実行します。

#### EXPOSE

ネットワーク上で通信を行なうための扉のようなものをポートといいます。

コンテナの内外はネットワーク上もつながっていないのでポート同士をつなげる必要があります。

`EXPOSE`命令は指定したポートを開放し、通信を待機します。

あとは`docker container run`のときにどのポート同士をつなげるかを指定してあげれば通信ができるようになります。

#### CMD

コンテナ実行時に呼び出すコマンドを指定します。

そのため、Dockerイメージのビルド時には実行されません。

さきほどのUbuntuのDockerコンテナを思い出してください。

```sh
$ docker container ls -a
CONTAINER ID   IMAGE            COMMAND       CREATED          STATUS                      PORTS     NAMES
405b5de9f51f   example:latest   "/bin/bash"   54 seconds ago   Exited (0) 53 seconds ago             epic_brown
```

この`COMMAND`というのが`CMD`命令にあたります。

`FROM ubuntu:latest` の`ubuntu`イメージの`CMD`命令が`"/bin/bash"`だったのです。

<https://github.com/tianon/docker-brew-ubuntu-core/blob/4b7cb6f04bc4054f9ab1fa42b549caa1a41b7c92/focal/Dockerfile#L43>

`RUN`はDockerイメージのビルド時に呼び出されます。

#### Docker image build2

ではこのDockerfileからDockerイメージをビルドしましょう。

```sh
$ pwd
/Users/taro/code/backenduniv/docker-training
$ docker image build rails-example -t rails-example:latest
[+] Building 3.9s (11/11) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 37B
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [internal] load metadata for docker.io/library/ruby:3.0
 => [auth] library/ruby:pull token for registry-1.docker.io
 => [1/5] FROM docker.io/library/ruby:3.0@sha256:091ee4779c0d90155b6d1a317855ce64714e6485f9db4413c812ddd112df7dc7
 => [internal] load build context
 => => transferring context: 57B
 => CACHED [2/5] WORKDIR /app
 => CACHED [3/5] COPY Gemfile /app/Gemfile
 => CACHED [4/5] COPY Gemfile.lock /app/Gemfile.lock
 => CACHED [5/5] RUN bundle install
 => exporting to image
 => => exporting layers
 => => writing image sha256:9130f1fc83abf08f4a1d19dcdcf492215d2cb03dbb05a075e974a35f9a5af8c1
 => => naming to docker.io/library/rails-example:latest
```

`EXPOSE 3000`までが完了しているDockerイメージができました。

#### Docker container run2

```sh
$ docker container run -p 3000:3000 -it rails-example:latest /bin/bash
root@104273e9384a:/app#
```

- `-p`はコンテナの内外のポート同士をつなげます。`-p {コンテナ内}:{コンテナ外}`です。
- `/bin/bash`はDockerfile内の`CMD`命令を上書きして、コマンドを実行します。実行できるコマンドであれば何でも構いません。(`ls`でも`pwd`でも)

Ruby on Railsプロジェクトを作成します。`bundle exec rails new .  --api`で実行してください。

ここでは何をやっているのか理解する必要はありません。

途中で`Overwrite /app/Gemfile?`と聞かれるので`Y`と入力してください。

```sh
root@5785c5749085:/app# bundle exec rails new .  --api
       exist
      create  README.md
      create  Rakefile
      create  .ruby-version
      create  config.ru
      create  .gitignore
      create  .gitattributes
    conflict  Gemfile
Overwrite /app/Gemfile? (enter "h" for help) [Ynaqdhm] Y
```

完了したらRuby on Railsアプリケーションのサーバーを立ち上げます。

`rails server -b 0.0.0.0`

```sh
root@5785c5749085:/app# rails server -b 0.0.0.0
=> Booting Puma
=> Rails 6.1.3.2 application starting in development
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.3.0 (ruby 3.0.1-p64) ("Sweetnighter")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 592
* Listening on http://0.0.0.0:3000
Use Ctrl-C to stop
```

ブラウザで<localhost:3000>を開いてみてください。

この画面が見えていれば、OKです！

<img src="./../public/docker/rails-hello.png" width="320" alt=" Yay! You’re on Rails!">

いつもどおり、`Ctrl+C`でサーバーを落として、コンテナから抜けましょう。

### Docker container rm

こんな感じで気軽にコンテナを立ち上げているとDockerイメージとDockerコンテナがどんどん増えていってしまいます。

ディスクがもったいないので削除しましょう。

まずはコンテナです。

`docker container ls -a`で状態を確認しましょう。

```sh
docker container ls -a
CONTAINER ID   IMAGE                  COMMAND       CREATED         STATUS                     PORTS     NAMES
a177765f04b0   rails-example:latest   "/bin/bash"   2 minutes ago   Exited (0) 2 minutes ago             stoic_chatelet
```

削除は`docker container rm`で実行できます。

削除には`CONTAINER ID`もしくは`NAMES`を使うことができます。

今回は`NAMES`を使います。

`CONTAINER ID`を使うときも`NAMES`を指定する箇所を置換してもらえばそのまま使うことができます。

```sh
$ docker container rm stoic_chatelet
stoic_chatelet
$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

削除されていることがわかります。

ちなみに`docker container run --rm`を使用すると、コンテナの終了時にコンテナの削除も一緒にしてくれます。

## Docker image rm

イメージも削除してみましょう。

`docker image ls -a`で確認しましょう。

```sh
$ docker image ls -a
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
rails-example   latest    b8be5a9da05f   26 minutes ago   906MB
example         latest    f2bed4af2163   2 weeks ago      72.7MB
ubuntu          latest    7e0aa2d69a15   2 weeks ago      72.7MB
```

削除には`docker image rm`を使用します。

```sh
$ docker image rm b8be5a9da05f
Untagged: rails-example:latest
Deleted: sha256:b8be5a9da05fe7d5d40a078cd8b02c5ace9aa4db318ee0af372bb5186d1d3fbb
$ docker image rm f2bed4af2163
Untagged: example:latest
Deleted: sha256:f2bed4af2163116084464018b01076094f252d4c7fee7bad02f8e73ef7a4e78d
$ docker image rm 7e0aa2d69a15
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:cf31af331f38d1d7158470e095b132acd126a7180a54f263d386da88eb681d93
Deleted: sha256:7e0aa2d69a153215c790488ed1fcec162015e973e49962d438e18249d16fa9bd
```

イメージの削除もできました。

```sh
$ docker image ls -a
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
```

ここでコミットしましょう！

```sh
$ git add .
$ git commit -m "add rails-example"
```

### Docker Compose

`docker compose`コマンドは、複数のコンテナで構成されるアプリケーションの実行をより簡単に行えるようにするツールです。

例えばデータベースを担うコンテナとRuby on Railsアプリケーションを担うDockerコンテナを一緒に立ち上げて、<br>
Ruby on Railsのコンテナからデータベースのコンテナにアクセスしたりできるようになります。


まず、`~/code/backenduniv/docker-training`に`compose-example`というディレクトリを作成しましょう。

```sh
$ mkdir ~/code/backenduniv/docker-training/compose-example
```

### docker-compose.yml

`docker compose`を使用すると、長くなりがちなDockerコマンドを短く保つことができます。

では見てきましょう。

`docker compose`コマンドには`Dockerfile`と別の`docker-compose.yml`ファイルが必要になります。

前回、`rails-example`イメージを作りました。

今回もこれを使用して説明していきます。

`compose-example`ディレクトリに`rails-example/Dockerfile`、`rails-example/Gemfile`、`rails-example/Gemfile.lock`をコピーしてください。

そして、`compose-example/docker-compose.yml`ファイルを作成してください。

```sh
$ cp rails-example/Dockerfile compose-example/Dockerfile
$ cp rails-example/Gemfile compose-example/Gemfile
$ cp rails-example/Gemfile.lock compose-example/Gemfile.lock
$ touch compose-example/docker-compose.yml
```

```sh
docker-training # 今いるディレクトリ
  └── compose-example
        ├── docker-compose.yml # 空のファイル
        ├── Dockerfile
        ├── Gemfile
        └── Gemfile.lock # 空のファイル
```

`compose-example/docker-compose.yml`を下記のように修正してください。

ここで理解する必要はありません。簡単に説明します。

```yml
version: '3.4'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
        - ./:/app:cached
    ports:
      - "3000:3000"
    command: /bin/bash -c '
        rm -f tmp/pids/server.pid &&
        bundle exec rails server -b 0.0.0.0
      '
```

これは`YAML`というフォーマットによって書かれています。

詳細には触れませんが、`YAML`が気になる場合は下記を参考にしてください。

[プログラマーのための YAML 入門 (初級編)](https://magazine.rubyist.net/articles/0009/0009-YAML.html)

```yml
services:
  app:
```

ここは`app`という名前の**サービス**(コンテナの設定)を定義しています。`app`を呼び出すだけで下に定義されている一連の設定を反映させることがきます。

```yml
    volumes:
        - ./:/app:cached
```

ここは`docker container run -v`で行っていたDockerコンテナにディレクトリをマウントする設定と同じです。

```yml
    ports:
      - "3000:3000"
```

ここは`docker container run -p`で行っていたコンテナの内外のポート同士をつなぐ設定と同じです。

```sh
    command: /bin/bash -c '
        rm -f tmp/pids/server.pid &&
        bundle exec rails server -b 0.0.0.0
      '
```

無くても動くのですが、[あるエラー](https://blog.cloud-acct.com/posts/u-rails-serverpid-error/)を防ぐためにあります。
理解する必要はありません。おまじないだと思ってもらえれば良いでしょう。

では、実際にコンテナを実行します。

ディレクトリを`compose-example`に変更します。

```sh
$ pwd
/Users/taro/code/backenduniv/docker-training
$ cd compose-example
$ ls
Dockerfile              Gemfile                 Gemfile.lock            docker-compose.yml
```

イメージのビルドも一緒にできます。

途中で`Overwrite /app/Gemfile?`と聞かれるので`Y`と入力してください。

```sh
$ docker compose run --rm app bundle exec rails new . --api
Creating network "compose-example_default" with the default driver
...
```

`docker compose run --rm app`でサービスを指定して実行します。

`bundle exec rails new . --api`がコンテナ内で実行するコマンドです。

Ruby on Railsアプリケーションを作成しています。

`docker`コマンドで実行する以下と同じです。

```sh
# これは実行しなくて良いです。
# docker-trainingディレクトリ上で
$ docker image build rails-example -t rails-example:latest
$ docker container run --rm -v $PWD/rails-example:/app -p 3000:3000 rails-example:latest bundle exec rails new . --api
```

`docker compose`を使ったほうが楽になると思ってもらえれば十分です。

`compose-example`ディレクトリはコンテナ内で自動生成された`Ruby on Rails`ディレクトリがマウントされているのでファイルがいっぱい増えています。

確認してみてください。

```sh
$ ls
Dockerfile              README.md               bin                     db                      log                     test
Gemfile                 Rakefile                config                  docker-compose.yml      public                  tmp
Gemfile.lock            app                     config.ru               lib                     storage                 vendor
```

`docker compose`でもサーバーを立ち上げてみます。

```sh
$ docker compose up
Docker Compose is now in the Docker CLI, try `docker compose up`

Creating compose-example_app_1 ... done
Attaching to compose-example_app_1
app_1  | => Booting Puma
app_1  | => Rails 6.1.3.2 application starting in development
app_1  | => Run `bin/rails server --help` for more startup options
app_1  | Puma starting in single mode...
app_1  | * Puma version: 5.3.0 (ruby 3.0.1-p64) ("Sweetnighter")
app_1  | *  Min threads: 5
app_1  | *  Max threads: 5
app_1  | *  Environment: development
app_1  | *          PID: 8
app_1  | * Listening on http://0.0.0.0:3000
```

ブラウザで<localhost:3000>を開いてみてください。

さきほどのRuby on Railsの画面が出ればOKです！

`docker compose`の力強いサポートを感じていただけたでしょうか?

`Ctrl+C`でコンテナを終了してください。

`docker compose down`でコンテナを削除できます。

```sh
$ docker compose down --volumes
```

下の定義でマウントしたファイルがDockerの管理下においてコンピュータ上に残っています。

```yml
    volumes:
        - ./:/app:cached
```

`--volumes`はそれを削除するためのフラグです。

以上でDocker composeの説明は終わりです！お疲れ様でした！

コミットをして終わりたいのですが、`Ruby on Rails`はプロジェクト作成時にデフォルトで`git init`します。

`docker-training/.git`がすでにあるので`docker-training/compose-example/.git`は削除しましょう。

```sh
$ pwd
/Users/taro/code/backenduniv/docker-training/compose-example
$ rm -rf .git
```

`rm -rf`は`rm`に`-r`と`-f`を合成したものです。

- `-r` はディレクトリの中にあるディレクトリもすべて対象にすることを意味します
  - これを再帰(Recursion)といいます
- `-f` は強制的に実行することを意味します

コミットしましょう。

```sh
$ git add .
$ git commit -m "add compose-example"
```

## おわりに

Dockerやコンテナ技術は毎日のように触れるものになると思います。Dockerを使ったツールやその他詳細はAppendixを参照してください。

ちなみにDockerで作成したすべてのもの<br>
(Dockerイメージ、Dockerコンテナ、Dockerボリューム、Dockerネットワーク)<br>
を削除したい場合は下記のコマンドで実行できます。

```sh
$ docker system prune -a --volumes
```

Dockerがうまく動かなかったり、ディスク容量を逼迫した場合はやってみてください。
