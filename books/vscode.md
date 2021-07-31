# Visual Studio Code

今まではインタラクティブな環境でSQLやRubyを書いてきました。

今までのコードは量が少ないのでそれで十分ですが、ソフトウェアエンジニアはそうはいきません。

これからはファイルに保存したRubyを実行していきます。

そのためにはファイルの編集を行なう、統合開発環境がかかせません。

## Visual Studio Code

Visual Studio Codeとは統合開発環境の一種です。

従来は呼ばれるプログラムを編集して保存するだけの機能しかない**テキストエディタ**を使って開発していました。

そのテキストエディタに便利な機能を多くくっつけたものが、**統合開発環境**です。

**IDE**(Integrated Development Environment)といいます。

現在でも、テキストエディタで開発している人もいます。何を選んでも基本的には自由です。

しかし、初心者の方々はなるべく、学習以外のノイズを減らしてほしいです。

そのため、もっとも使いやすく、シェアの高いIDEであるVS Codeを選ぶことは非常に自然なことです。

どんなテキストエディタやIDEがあるのか、ぜひ調べてみてください。

## VS Codeのインストール

VSCodeを下記のリンクからインストールします。下記のリンクからダウンロードしてください。

[Visual Studio Code](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)

ダウンロードが完了したら、zipファイルなので解凍してください。

解凍が完了したら、アプリケーションフォルダにドラッグアンドドロップしてください。完了です。

起動してください。

下記のような画面が表示されていると思います。

![VS Code](./../public/vscode/vs-code.png)

## セットアップ

いったんVS Codeから離れてターミナルの操作に移りましょう。

`~/code/burning-tech`に移動してください。

```sh
$ cd ~/code/burning-tech
```

教材リポジトリをクローンします。

```sh
$ git clone https://github.com/burning-tech/vscode-training.git
$ cd vscode-training
```

本チャプターでもいつものように適宜、`git`管理をしてください。

```
$ rm -rf .git
$ git init
```

## Dockerの復習

cloneしたリポジトリには`docker-compose.yml`および、`Dockerfile`が用意されています。

復習も兼ねて確認してみましょう。

```sh
$ cat Dockerfile
FROM ruby:3.0.1

$ cat docker-compose.yml
version: '3.4'

services:
  app:
    build: .
    volumes:
        - ./:/app:cached
    working_dir: /app
    command: bash
```

DockerfileはRubyイメージの`FROM`命令のみが記載されています。

`docker-compose.yml`の`build .`の`.`はカレントディレクトリ(現在のディレクトリ、つまり`~/code/burning-tech/vscode-training`)を示します。

カレントディレクトリから`Dockerfile`を探し、見つけたらそれを元にDockerイメージをビルドします。

```sh
    volumes:
        - ./:/app:cached
```

上記の`./`はカレントディレクトリを示し、`/app`はDockerコンテナ内の`/app`ディレクトリを示します。

両者のファイルが共有されるように命令します。

Dockerコンテナ内での`/app`でファイルを書き換えても、Dockerコンテナ外の`~/code/burning-tech/vscode-training`でファイルを書き換えても、お互いにファイルが共有されることになります。

コンテナを消しても、ローカル(`Docker`コンテナの外)のファイルは残ります。

```sh
    working_dir: /app
    command: bash
```

これは`/app`で`bash`というコマンドを実行するという意味になります。

今後は、コンテナ内とコンテナ外を上手く行来しながら、プログラムを書いていきます。

## 基本操作

それではVS Codeの操作に戻りましょう。

### ワークスペース

`Cmd`+`O`キーを押下して、作業を行なう`~/code/burning-tech/vscode-training`ディレクトリを開いてください。

この作業を行なうディレクトリを**ワークスペース**といいます。

開いたらそのディレクトリにどんなファイルがあるか左側に表示されているはずです。

![File Tree](./../public/vscode/file-tree.png)

これを**ファイルツリー**といいます。

ファイルツリーに表示されているファイルをクリックするとファイルが`VS Code`に表示され、編集できるようになります。

初心者は**ファイルツリー**からファイルの編集を行なうことがほとんどだと思うので、`VS Code`を色々いじってみたり、調べてみてください。

下記で示す箇所をクリックすると非表示にできます。

![Hide](./../public/vscode/hide-file-tree.png)

### ファイルの作成

`Cmd` + `N`キーを押下してください。

入力画面に移ったと思います。

![New file](./../public/vscode/new-file.png)

適当に入力して保存してみましょう。

`Cmd` + `S`キーを押下してください。


![Save](./../public/vscode/save.png)

どんな名前でどこに保存するかを選んで保存してください。

基本的な操作はこれだけです。

ファイルツリーのファイル上でダブルクリック、右クリック等々してどんな操作ができるのか色々と試行錯誤しながら試してみてください。

- 削除
- ディレクトリ作成
- ディレクトリ移動
- リネーム

今は上記ができれば十分です。

### ファイル検索

ファイルツリーでの操作は非常にわかりやすいのものです。

しかし、ファイル数が多く、かつワークスペースのプログラムについて熟知している場合は面倒な作業になります。

そこでファイル名で検索して表示をしてみましょう。

`Cmd` + `P`キーを押下してください。

![File Search](./../public/vscode/file-search.png)

入力バーが表示されるのでここに`docker`と入力してみましょう。

![File Search Docker](./../public/vscode/file-search-with-docker.png)

`docker`にマッチしたファイルが表示されました。

`Enter`キーで編集画面に移ることができます。

### キーワード検索

ファイルの中に記述されているキーワードも検索できます。

`Cmd` + `Shift` + `F`キーを押下してください。

出てきた入力バーに`app`と入れてみましょう。マッチしたファイルが表示されます。

![Search Keyword](./../public/vscode/search-keyword.png)

単一のファイル内だけで検索したい場合は、ファイルを開き、`Cmd` + `F`キーでできます。

### Remote - Containers

Dockerを使用した開発をより便利なものにする拡張機能というものがあります。

そのうちのひとつの`Remote Containers`は非常に強力です。

Dockerコンテナ内の中でVS Codeを直接開き、開発できます。

下記のリンクを開き、`Install`をクリックしてください。

[Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

VS Codeが下記のような画面になりますので再度、`Insall`をクリックしてください。

![Install Remote Container](./../public/vscode/install-remote-container.png)

インストールが完了しました。

VS Codeに下記画像の赤枠で囲ったボタンが表示されるのでクリックしてください。

![Remote Container](./../public/vscode/remote-container.png)

下記のような画面になります。これがRemote Containerの操作メニューです。

![Remote container menu](./../public/vscode/remote-container-menu.png)

`Open Folder in Container`をクリックしてください。

![Open Folder in Container](./../public/vscode/open-folder-in-container.png)

`Open`を押すと下記のようにどのようにコンテナを起動するか選択する画面になります。

![From Docker Compose](./../public/vscode/from-docker-compose.png)

必ず、`From docker-compose.yml`をクリックしてください。

これを選択することでワークスペースにある`docker-compose.yml`を元にDockerコンテナを起動してくれます。

Dockerコンテナの起動中は下記のよう画面になります。

![Start Container](./../public/vscode/start-container.png)

起動が完了すると、下記のようにファイルツリーが表示されます。

![File Tree in Container](./../public/vscode/file-tree-in-container.png)

今見えているファイルはコンテナ内のファイルです。

起動と同時に`.devcontainer`ディレクトリが作成されます。

これはVS Codeを一度閉じて、もう一度作業を開始するときなどに今まで`docker-compose.yml`を元にコンテナを立ち上げるように選択してきたフローを自動的にやってくれるようになります。

一度やってみましょう。

左下の`Dev Container: Existing Docker Compose (...`をクリックしてください。

![Dev Container](./../public/vscode/dev-container-existing.png)

`Close Remote Connection` をクリックしてください。

![Close Remote Container](./../public/vscode/close-remote-container.png)

`Remote`というのはここではコンテナ内を示しています。

(`Remote`がコンテナ内、`Local`がコンテナ外)

コンテナの外に戻ってきました。

Remote Containerのボタンをクリックして、メニューを開くと、`vscode-training`というコンテナがあることがわかります。

![Still exist](./../public/vscode/container-still-exists.png)

これはさきほど起動していたコンテナです。

一度、削除しましょう。

`vscode-training`にマウスカーソルを合わせると`X`ボタンが表示されますので削除します。

![Remove button](./../public/vscode/remove-container-button.png)

`Remove`をクリックしてください。

![Remove button](./../public/vscode/remove-container.png)

コンテナが削除されました。

![Not found](./../public/vscode/container-already-not-found.png)

もう一度、コンテナを立ち上げてみましょう。

コンテナから外に出ることで、ワークスペースが、解除されています。

`Cmd` + `O`で再度、`~/code/burning-tech/vscode-training`を開いてください。

すると、下記のように右下にインフォメーションが表示されます。

![Re open](./../public/vscode/reopen-in-container.png)

`Reopen in Container`をクリックしてください。

さきほど自動で生成された`.devcontainer`を元にコンテナが起動しました。

今後、同様の操作を行なう際は紹介した方法で起動してください。

### より詳しく

テキストエディタに詳しくなって、使いこなせるようになると、エンジニアリングの生産性は飛躍的に向上します。

これ以降、基本的な操作(ファイル作成、保存等)は逐一説明しませんが、 わからないことがあっても、安心してください。

あなたには`Google`があります。ぜひ、いろいろなことを調べてみてください。

Appendixに、おすすめの拡張機能を記載していますので、この先に進む前にすべて入れておいたほうが良いでしょう。

## はじめてのファイル実行

それではRubyプログラムをファイルに書いていきましょう。

まずは、ファイルを作成します。

```sh
touch hello.rb
```

`VS Code`で作成しても構いません。

作成したファイルを`VS Code`で開いてください。

![First ruby file](./../public/vscode/first-ruby-file.png)

`Hello`と出力するRubyプログラムを書いてみましょう。

```rb
puts 'Hello'
```

![Hello](./../public/vscode/hello.png)

では、実行してみましょう。

ターミナルから、Rubyプログラムを実行します。

VS Codeの画面はそのままで、ターミナルの操作に写ってください。

Rubyプログラムの実行は`ruby ファイル名`で実行できます。

`ruby`というコマンドがファイルを読み取って実行してくれます。

ではコンテナの中にある`ruby`を呼び出してファイルを実行してみましょう。

念の為、カレントディレクトリが`~/code/burning-tech-dev/vscode-training/`であることを確認してください。

```sh
$ pwd
/Users/taro/code/burning-tech-dev/vscode-training
```

```sh
$ docker compose exec app ruby hello.rb
```

`docker compose exec`はすでに動いているコンテナに対して何かを実行するコマンドでした。

`docker-compose.yml`に記載されている`app`に対して`ruby hello.rb`というコマンドを実行するという意味です。

あなたのコンピュータ内に`ruby`がインストールされていれば(されていることでしょう)、ローカルでも直接実行できます。

```sh
$ ruby hello.rb
Hello
```

できるなら、ローカルでいいじゃないか！と思うかもしれませんが、実際の開発では、ローカルと、コンテナの中にはどんどん差分が生まれていきます。今でもおそらく、ローカルにインストールされている`ruby`のバージョンは異なるでしょう。

```sh
$ docker compose exec app ruby -v
ruby 3.0.1p64 (2021-04-05 revision 0fb782ee38) [x86_64-linux]
$ ruby -v
ruby 2.6.3p62 (2019-04-16 revision 67580) [universal.x86_64-darwin19]
```

私のコンピュータ上の`ruby`のバージョンは`2.6.3`でした。きっと皆さんのコンピュータ上の`ruby`のバージョンとも異なるでしょう。

ですが、コンテナの`ruby`のバージョンは`3.0.1`です。これはあなた達のコンテナも同じはずです。

実際に成果物としてデプロイの対象となるのはコンテナ(および、Dockerイメージ)です。


ローカルでは動いても、コンテナで動かなければ意味がありません。コンテナ内で実行を行なうことを心がけましょう。

いちいち`docker compose exec app`と入力するのが嫌だなあと言うのであればコンテナの中に入りっぱなしで作業を行なうのが良いでしょう。

```rb
$ docker compose exec app bash
root@cbed598d45de:/app# ruby hello.rb
Hello
root@cbed598d45de:/app#
```

## おわりに

Rubyのファイル実行をVS Codeおよび、Docker、Remote Containerとともに学びました。

次回以降はより実践的なプログラムを開発していきます。

本日作成したコンテナ等は今後使用しないため、削除して構いません。

(削除方法はわかりますね？)

VS Codeのより詳細な設定はAppendixを参照してください。
