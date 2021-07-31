---
title: "テストコードを書きながら開発しよう | RSpecの基本的な書き方"
---

# RSpec

ここからは、いままでの小さなプログラムではなく、より複雑なプログラムを書く準備をしていきます。

## セットアップ

`~/code/backenduniv`に移動してください。

```sh
$ cd ~/code/backenduniv
```

教材リポジトリをクローンします。

```sh
$ git clone https://github.com/backenduniv/rspec-training.git
$ cd rspec-training
```

最終的な成果物は`ans`ディレクトリにありますので動かなかったり、わからない箇所があれば参考にしてみてください。

本チャプターでもいつものように適宜、`git`管理をしてください。

```sh
$ rm -rf .git
$ git init
```

これに合わせてカレントディレクトリをVS Codeで開き、Remote Containerでコンテナを立ち上げてください。

## Fizz buzz

[Fizz buzz](https://en.wikipedia.org/wiki/Fizz_buzz)という算数のゲームがあります。

アメリカのコーディング面接の際にこの問題をプログラムできた人が、200人のうち1人だけだったという有名な記事があります。

[Don't Overthink FizzBuzz](http://weblog.raganwald.com/2007/01/dont-overthink-fizzbuzz.html)

この記事がもととなってプログラミングの世界で非常に有名になったそうです。

この問題をみなさんと一緒に解いていきます。

難しいのではないかと思うかもしれませんが、安心してください。

今まで学習したRubyの知識のほとんどでこの問題は解くことができます。

Fizz buzz問題はこのようなものです。

> 1 から 100 までの数字を出力するプログラムを作成します。
ただし、3の倍数には数字の代わりに「Fizz」を出力し、5の倍数には「Buzz」を出力します。
3と5の両方の倍数である数の場合は「FizzBuzz」を印刷します。

難しいと感じたかもしれません。(少し時間を取ってやってもらっても構いません。)

この問題を解けたかどうかを判定するために**ユニットテスト**を導入したいと思います。

## ユニットテスト

[単体テスト](https://ja.wikipedia.org/wiki/%E5%8D%98%E4%BD%93%E3%83%86%E3%82%B9%E3%83%88)

ユニットテストとはプログラムの小さな関数や、クラスごとにその動作が正しいかを確認するために作成するものです。

ユニットテストはいちいち人がプログラムを実行することで行なうこともできます。

ですが、ここではテストの自動化を行って、何度も簡単にユニットテストを実行する方法を学びましょう。

これを**テスト自動化**といいます。

[Test automation](https://en.wikipedia.org/wiki/Test_automation)

自動化するためには、ユニットテストもプログラムである必要があります。

自動テストプログラムを実行することで、ユニットテストが行われ、プログラムが正しいか間違っているかを判定します。

つまりこれから2つのプログラムをコーディングしていきます。

1. Fizz Buzzプログラム
2. FIzz Buzzのプログラムが正しいかを検証するユニットテストプログラム

## 最小のプログラム

Fizz Buzzプログラムに立ち向かう前にもっと簡単なプログラムを書いて、そのテストを書いてみましょう。

> 文字列`Hello`を返す関数`get_hello`を実装してください。

これはみなさんなら簡単でしょう。

カレントディレクトリに`hello.rb`を作成してください。

`hello.rb`を下記のように編集してください。

```rb
def get_hello
  'Hello'
end
```

以上です。

ではこの関数を実行し、出力を行なう行を追加します。

```diff
  def get_hello
    'Hello'
  end
+
+ puts get_hello
```

`+`で書かれた行は今までのコードに追加したコードを意味します。

つまり一行の空白行と`puts get_hello`の実行の行をもとのプログラムに追加しています。

`puts get_hello`は`get_hello`の実行結果(`Hello`)を出力します。

では実行しましょう。

```sh
$ docker compose exec app ruby hello.rb
Hello
```

`Hello`の出力を確認できました。

私達は`get_hello`関数が`Hello`を返すことをテストできました。

今行ったテストには3つの工程が含まれています。

1. プログラムを実行する
2. 目視で結果を確認する
3. 結果が正しいか判定する

今回は返却する値が`Hello`のみでしたが、前回作成した円柱の体積の関数だった場合どうなるでしょうか?

`3`の工程に非常に時間がかかるはずです。

なので、自動テストを追加しましょう。

## 最小の自動テスト

目視で出力することをやめて、自動テストを作成します。

下記のようにプログラムを修正してください。


```diff
  def get_hello
    'Hello'
  end

- puts get_hello
+ def test_get_hello
+   actual = get_hello
+   expect = 'Hello'
+   if actual == expect then
+     puts 'OK'
+   else
+     puts 'NG'
+   end
+ end
+
+ test_get_hello
```

`-`は削除した行を意味しています。つまり、`puts get_hello`を削除しています。

このような`-`と`+`で出力を示すのは[diff](https://ja.wikipedia.org/wiki/Diff)という２つのファイルの差分を出力できるコマンドの出力からきています。

追加した関数を確認しましょう。

```rb
def test_get_hello
  actual = get_hello
  expect = 'Hello'
  if actual == expect then
    puts 'OK'
  else
    puts 'NG'
  end
end
```

まず、`get_hello`を実行し、`actual`という変数に代入しています。

そして、今回`get_hello`の結果の期待値である`Hello`を`expect`変数に代入しています。

つぎの`if`では`actual`と`expect`を比較し、同じ文字列であれば`OK`を出力し、そうでなければ`NG`を出力します。

つまり、この`test_get_hello`を実行し、`OK`と出力されれば`get_hello`は正しいプログラムといえます。

実行してみましょう。

```rb
$ docker compose exec app ruby hello.rb
OK
```

うまく行っているようです！

`get_hello`にバグをいれて、テストが失敗することも確認してみましょう。

下記のようにプログラムを修正して、プログラムを再度実行してみましょう。

```diff
  def get_hello
-   'Hello'
+   'Hillo'
  end

  def test_get_hello
    actual = get_hello
    expect = 'Hello'
    if actual == expect then
      puts 'OK'
    else
      puts 'NG'
    end
  end

  test_get_hello
```

```rb
$ docker compose exec app ruby hello.rb
NG
```

失敗していることも確認できました！

自動テストを追加することで工程が下記のように変わりました。

1. プログラムを実行する
2. 結果が正しいか***確認***する

プログラムの正しさを判定する工程をプログラムそばに寄せることでプログラムの動作の確認を容易にすることがきました。

他にも自動テストには良いことがあります。

1. プログラムを修正したときにそのプログラムが正しいままかどうか容易にわかる
2. そのプログラムの動作を自動テストのコードから読み取れる

このような理由から自動テストはソフトウェアの世界において非常に重要だと考えられています。

では、テストコードとプログラムを分離しましょう。

まずは`hello.rb`からテストを削除します。(`get_hello`を元に戻すのを忘れないでください！)

```diff
  def get_hello
-   'Hillo'
+   'Hello'
  end
-
- def test_get_hello
-   actual = get_hello
-   expect = 'Hello'
-   if actual == expect then
-     puts 'OK'
-   else
-     puts 'NG'
-   end
- end
-
- test_get_hello
```

テストを行なうためだけのファイル`hello_test.rb`を作成しましょう。

`hello_test.rb`を下記のように編集してください。

```rb
require "./hello.rb"

def test_get_hello
  actual = get_hello
  expect = 'Hello'
  if actual == expect then
    puts 'OK'
  else
    puts 'NG'
  end
end

test_get_hello
```

`require "ファイル名"`と記載することでそのファイルの中に定義された関数、変数、クラス等を使用できます。

このようにファイルを分割することでファイルの持つ責務が明確になります。

- `hello.rb` => `get_hello`メソッドを定義する
- `hello_test.rb` => `get_hello`メソッドをテストする

一度テストが実行できるか確認してみましょう。

```sh
$ docker compose exec app ruby hello_test.rb
OK
```

`require`によって、`get_hello`を呼び出すことができました。

`require "./hello"`のように拡張子を省略できます。

```sh
$ docker compose exec app ruby hello_test.rb
<internal:/usr/local/lib/ruby/3.0.0/rubygems/core_ext/kernel_require.rb>:85:in `require': cannot load such file -- ./hello.rb (LoadError)
        from <internal:/usr/local/lib/ruby/3.0.0/rubygems/core_ext/kernel_require.rb>:85:in `require'
        from hello_test.rb:1:in `<main>'
```

上記のようなエラーが発生する場合は、同一のディレクトリに`hello.rb`と`get_hello.rb`がない可能性があります。

確認してみてください。

```
rspec-training
├── hello.rb
└── hello_test.rb
```

## minitest

Rubyには公式で`minitest`というユニットテストを行なうためだけのライブラリが用意されています。

ライブラリとは特定の機能を持ったコードをひとまとめにして、他のプログラムから呼び出して使用できるようしたものです。

`minitest`を使って`test_get_hello`を書き直してみましょう。

`hello_test.rb`を下記のように修正します。

```diff
+ require 'minitest/autorun'
  require './hello.rb'

- def test_get_hello
-   actual = get_hello
-   expect = 'Hello'
-   if actual == expect then
-     puts 'OK'
-   else
-     puts 'NG'
-   end
- end

- test_get_hello

+ class TestHello < MiniTest::Unit::TestCase
+   def test_get_hello
+     assert_equal "Hello", get_hello
+   end
+ end
```

一行目の`require 'minitest/autorun'`で`minitest`ライブラリを使用できるようにします。

つまり`require`はファイル名かライブラリのどちらかを指定できます。

続いて`TestHello`というクラスを定義しています。

```rb
class TestHello < MiniTest::Unit::TestCase
  def test_get_hello
    assert_equal "Hello", get_hello
  end
end
```

`TestHello`クラスは`MiniTest::Unit::TestCase`クラスを継承することでテストコードとして機能します。

`::`は絶対指定と呼ばれる記法で、クラスや、モジュール内に定義されたクラスを指定するときにこのように記載します。

```rb
module Minitest
  class Unit
    class TestCase
      ...
    end
  end
end
```

この`MiniTest::Unit::TestCase`クラスの定義は下記のリンクから実際のコードを見ることができます。

<https://github.com/seattlerb/minitest/blob/master/lib/minitest/unit.rb#L20-L43>

クラス内にクラスを定義できることや、モジュールについては今は詳しく知る必要はありません。

`TestCase`クラスを`TestHello`は継承しているんだということが理解できれば問題ありません。

`MiniTest::Unit::TestCase`クラスを継承したクラスは、実装したメソッドを自動的に実行してくれます。

`test_get_hello`が実際のテストコードです。

```rb
def test_get_hello
  assert_equal "Hello", get_hello
end
```

`assert_equal`メソッドは第1引数に期待値、第2引数に検証したい値を受け取ります。

２つの値が正しい場合は、テストにパスします。

では`hello_test.rb`を実行してみましょう。

```sh
$ docker compose exec app ruby hello_test.rb
MiniTest::Unit::TestCase is now Minitest::Test. From hello_test.rb:4:in `<main>'
Run options: --seed 64959

# Running:

.

Finished in 0.001356s, 737.6263 runs/s, 737.6263 assertions/s.

1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```

出力された最終行を見てください。

```sh
1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```

1. ひとつのテストが実行
2. ひとつのアサーションが実行
3. 失敗はなかった
4. エラーはなかった
5. スキップされたテストはなかった。

つまりテストにパスしたことがわかります。

`MiniTest::Unit::TestCase`が裏で色々やってくれていてこのような分かりやすい出力を見ることができるとわかれば良いでしょう。

## アサーション

[アサーション](https://ja.wikipedia.org/wiki/%E8%A1%A8%E6%98%8E_(%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0))とはプログラムにおける前提条件チェックします。

- `1 + 1`の値は`3`です。
- `3 * 2`の値は`6`です。
- `6 - 4`の値は`2`です。

というようにその前の値が正しいことを前提にしてプログラムは実行していきます。

`TestHello`クラスの`test_get_hello`メソッドは`get_hello`の返り値が`Hello`という前提条件をチェック(テスト)しています。

このチェックに違反している場合、プログラムはそれ以降の処理を正しく実行できないため、Rubyはプログラムを即時に実行停止にするため、**例外**を発生させます。

## 例外

**例外**(**Exception**)もRubyでは、クラスで実装されています。

[class Exception](https://docs.ruby-lang.org/ja/latest/class/Exception.html)

例外は下記のように発生させることができます。

```rb
raise Exception
```

`get_hello`メソッドを修正して例外を発生させてみましょう。

`hello.rb`を下記のように修正してください。

```diff
  def get_hello
+   raise Exception
    'Hello'
  end
```

テストを実行してみましょう。

```sh
$ docker compose exec app ruby hello_test.rb
MiniTest::Unit::TestCase is now Minitest::Test. From hello_test.rb:4:in `<main>'
Run options: --seed 14506

# Running:

E

Finished in 0.001782s, 561.0413 runs/s, 0.0000 assertions/s.

  1) Error:
TestHello#test_get_hello:
Exception: Exception
    /app/hello.rb:2:in `get_hello'
    hello_test.rb:6:in `test_get_hello'

1 runs, 0 assertions, 0 failures, 1 errors, 0 skips
```

最終行を見るとエラーが発生していることがわかります。

その上には例外がどこで発生したかがわかるように表示されています。

下から上がるように見ていってください。

1. ``hello_test.rb:6:in `test_get_hello'`` => `hello_test.rb`の6行目`test_get_hello`で例外が発生しているとわかります。
2. ``/app/hello.rb:2:in `get_hello'`` => `/app/hello.rb`の2行目`get_hello`で例外が起きているとわかります。

`hello.rb`の2行目は`raise Exception`を記述したので正しいことがわかります。

`0 assertions`となっているのは例外が発生したため、`assert_equal`の実行が行われなかったということです。

例外が即時に実行停止を行なうことがわかります。

例外は非常に重要な概念なので今後もとりあげていきます。

```sh
    /app/hello.rb:2:in `get_hello'
    hello_test.rb:6:in `test_get_hello'
```

のような下から上に例外の発生地点へ向かっていくような表示を**スタックトレース**といいます。

スタックトレースをきちんと読めるようにならないと、起こっている例外を正しく認識できません。

理科しておきましょう。

## RSpec

`minitest`の説明はここまでです。本講座でも今後使いません。

なぜなら、業務レベルのテストコードでは`Ruby`に標準で組み込まれている`minitest`を抑えてシェアを獲得しているテスティングライブラリがあります。

[RSpec](https://rspec.info/)

`minitest`だと書くのが難しいテストコードを`RSpec`だと用意に書くことができます。

では、`minitest`で書いたテストを`RSpec`で書き直します。

## gem

`RSpec`はサードパーティ(公式ではない)ライブラリです。Rubyではサードパーティライブラリを***gem***といいます。

そのため、現在起動しているコンテナには含まれていません。

なので`RSpec`は別途インストールする必要があります。

`gem install`コマンドで`gem`をインストールできるのですが、基本的には使用しません。

アプリケーションの開発において、ライブラリのバージョンの管理が必要になります。

それぞれのメンバー間でライブラリのバージョンが違うことはバグを生むことに繋がりかねません。

## Bundler

そのため、`gem`のバージョン管理するツールである`Bundler`を使用します。

`Bundler`をしようすることですべてのコンピュータ上で同一の`gem`をインストールできます。

[Bundler](https://bundler.io/)

`bundler`はそのシェアの高さから、Default gemsとして`ruby`に標準添付されています。

[Ruby 2.6.0-preview3 Released](https://www.ruby-lang.org/ja/news/2018/11/06/ruby-2-6-0-preview3-released/)

そのため、すでに起動中のコンテナで使用できます。

`bundle`コマンドで動作します。バージョンから確認してみましょう。

立て続けにコマンドを実行していくことになるので、コンテナの中に入って作業しましょう。

```sh
$ docker compose exec app bash
root@6feae3e7a072:/app#
```

`bundle -v`でバージョンを確認できます。

```rb
root@6feae3e7a072:/app# bundle -v
Bundler version 2.2.15
```

標準添付されていることを確認できました。

`git`のように`bundle init`を実行することで、カレントディレクトリをを`Bundler`で管理できます。

```sh
root@6feae3e7a072:/app# bundle init
Writing new Gemfile to /app/Gemfile
```

`gem`を記載することで管理対象をしめすことができる`Gemfile`が作成されました。

`VS Code`で確認してみましょう。

カレントディレクトリの`Gemfile`を`VS Code`で開いてください。

```Gemfile
# frozen_string_literal: true

source "https://rubygems.org"

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

# gem "rails"
```

すでに記載されているコードの意味を現在は理解する必要はありません。

`#`の行はコメントで、`Bundler`には無視されます。

気になる場合は下記を参考してください。

[Rails 5.0.1 で rails new するとできる Gemfile の git_source ブロックの意味](https://blog.kymmt.com/entry/rails-5.0.1-gemfile)

`Gemfile`を下記のように修正してください。


```diff
  # frozen_string_literal: true

  source "https://rubygems.org"

  git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

  # gem "rails"

+ gem "rspec"
```

`gem "rspec"`を記載することで、`rspec`をインストールすることを`bundler`に示すことができます。

`Gemfile`を元に`gem`のインストールを行なうには`bundle install`を実行します。

```rb
root@6feae3e7a072:/app# bundle install
Fetching gem metadata from https://rubygems.org/...
Resolving dependencies...
Using bundler 2.2.15
Fetching diff-lcs 1.4.4
Fetching rspec-support 3.10.2
Installing rspec-support 3.10.2
Installing diff-lcs 1.4.4
Fetching rspec-core 3.10.1
Fetching rspec-expectations 3.10.1
Fetching rspec-mocks 3.10.2
Installing rspec-mocks 3.10.2
Installing rspec-expectations 3.10.1
Installing rspec-core 3.10.1
Fetching rspec 3.10.0
Installing rspec 3.10.0
Bundle complete! 1 Gemfile dependency, 7 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

記載された`gem`(ここでは`Rspec`および、`Rspec`が内部で使用している`gem`)がインストールされました。

それに合わせて`Gemfile.lock`というファイルが作成されました。確認してみましょう。

```
GEM
  remote: https://rubygems.org/
  specs:
    diff-lcs (1.4.4)
    rspec (3.10.0)
      rspec-core (~> 3.10.0)
      rspec-expectations (~> 3.10.0)
      rspec-mocks (~> 3.10.0)
    rspec-core (3.10.1)
      rspec-support (~> 3.10.0)
    rspec-expectations (3.10.1)
      diff-lcs (>= 1.2.0, < 2.0)
      rspec-support (~> 3.10.0)
    rspec-mocks (3.10.2)
      diff-lcs (>= 1.2.0, < 2.0)
      rspec-support (~> 3.10.0)
    rspec-support (3.10.2)

PLATFORMS
  x86_64-linux

DEPENDENCIES
  rspec

BUNDLED WITH
   2.2.15
```

`Gemfile.lock`には、`Gemfile`だけでは不十分な`gem`が依存する`gem`などすべてがバージョン情報を合わせて記載されます。

`Gemfile`と`Gemfile.lock`を使うことで完全に同一のバージョンの`gem`を何度も、別の環境にインストールできます。

なので、`Gemfile`も`Gemfile.lock`も`git`管理の対象です。

## Dockerfile

このままでは、コンテナを削除し、再度実行した際に、手作業で`bundle install`を実行しなければなりません。

これらの手順はDockerイメージのビルドと同時に行なうべきです。

`Dockerfile`を修正します。

```diff
  FROM ruby:3.0.1

+ WORKDIR /app
+
+ COPY Gemfile /app/Gemfile
+ COPY Gemfile.lock /app/Gemfile.lock
+
+ RUN bundle install
```

今までは`docker-comopse.yml`でワーキングディレクトリを設定していました。

しかし、`docker-compose.yml`の記載はイメージのビルド時ではなく、コンテナの起動時に行われます。

なのでイメージのビルド時にワーキングディレクトリを設定したい場合は`Dockerfile`に記載する必要があります。

`WORKDIR /app`でコンテナ内の`/app`をワーキングディレクトリに設定しています。

```Dockerfile
COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock
```

コンテナ外の`Gemfile`と`Gemfile.lock`をコンテナ内の`/app/Gemfile.lock`と`/app/Gemfile.lock`にコピーしています。

```Dockerfile
RUN bundle install
```

`bundle install`を実行しています。

では、再度イメージをビルドしてコンテナを立ち上げてみましょう。

`VS Code`から実行できます。

左下の`Dev Container: Existing Docker Compose (...`をクリックしてください。

![Rebuild Container](https://res.cloudinary.com/dpnkkfkox/image/upload/v1627719666/rebuild-container_jputcn.png)

`Rebuild Container`をクリックしてください。

再度ビルドが行われ、`bundle install`が行われた状態でコンテナが立ち上がりました。

`Gemfile`に記載された`gem`がインストールされているか確認するには`bundle check`を実行します。

(※コンテナのリビルドで、もとのコンテナは削除されていますのでターミナル上でコンテナ内に入っていてもローカルに戻っています。)

```sh
$ docker compose exec app bundle check
The Gemfile's dependencies are satisfied
```

問題なくインストールされているようです。

インストールされていない場合、下記のように表示されます。

```sh
The following gems are missing
 * diff-lcs (1.4.4)
 * rspec-support (3.10.2)
 * rspec-core (3.10.1)
 * rspec-expectations (3.10.1)
 * rspec-mocks (3.10.2)
 * rspec (3.10.0)
Install missing gems with `bundle install`
```

`Bundler`でインストールしたgemのコマンドは`bunlde exec コマンド名`で呼び出すことができます。

## Setup RSpec

`RSpec`でテストコードを書く前にディレクトリを`rspec --init`で`RSpec`用に初期化します。

```sh
$ docker compose exec app bundle exec rspec --init

  create   .rspec
  create   spec/spec_helper.rb
```

`rspec --init`を`bundle exec`で呼び出しています。

`.rspec`と`spec/spec_helper.rb`が生成されました。

`.rspec`については`RSpec`が解釈する設定なのでここでは理解する必要ありません。

`spec/spec_helper.rb`は`RSpec`をどのように動作させるかを設定するためのファイルです。

デフォルトでも色々書いてあって、そのまま使えるのですが、より便利に使うための設定が`=begin`と`=end`に囲まれた部分に記載されています。

`=begin`と`=end`の間はコメントとして`Ruby`に解釈されるので実行されません。

なので`=begin`と`=end`を削除して、実行されるようにします。

`spec/spec_helper.rb`を下記のように修正してください。(長いので省略しています。)

```diff
  ...
  # The settings below are suggested to provide a good initial experience
  # with RSpec, but feel free to customize to your heart's content.
- =begin
    # This allows you to limit a spec run to individual examples or groups
    # you care about by tagging them with `:focus` metadata. When nothing
  ...
    # test failures related to randomization by passing the same `--seed` value
    # as the one that triggered the failure.
    Kernel.srand config.seed
- =end
  end
```

これでテストを書く準備が整いました。

## RSpec2

それでは、`RSpec`でユニットテストを書き直してみましょう。

`RSpec`のテストは`spec`ディレクトリに配置する規約があります。そうでないと実行されません。

なので、`hello_test.rb`はそのままにしておいて、`spec/hello_spec.rb`を作成します。

```sh
$ touch spec/hello_spec.rb
```

`VS Code`で`spec/hello_spec.rb`を開いて、下記のように修正してください。

```rb
require 'spec_helper'
require './hello'

RSpec.describe 'Hello' do
  it "is equal Hello" do
    expect(get_hello).to eq "Hello"
  end
end
```

`require 'spec_helper'`で`RSpec`を利用できるようになります。

下記のような書き方は`minitest`同様に`RSpec`がテストを実行し、どのように結果を出力するかを裏でコントロールするための記法です。

```rb
RSpec.describe 'テスト対象' do
  it "is expected value" do
    ...
  end
end
```

裏でどのように動くのかをここで理解する必要はありません。

ここで理解するべきは`RSpec.describe ~ do`はテストの対象(クラスの場合はここにクラスを記述します)を示し、`it ~ do`はテストケースを示すということだけです。

そのテストがなんのテストなのか、わかるように記述することを心がけましょう。

[GitLab](https://gitlab.com/gitlab-org/gitlab)でも`RSpec`を採用しているので参考にしてみてください。

[gitlab/spec/lib/gitlab/auth/current_user_mode_spec.rb](https://gitlab.com/gitlab-org/gitlab/-/blob/master/spec/lib/gitlab/auth/current_user_mode_spec.rb)

下記はテストケースによって書き方が異なってきます。やりたいことによって逐一調べていくのが良いでしょう。

`expect()`にはテストケースで対象となるプログラムの結果を引数として渡します。

そして、その結果と特殊なメソッドを持ったオブジェクトを返却します。

その特殊なメソッドの一つが`to`です。

`to`はその後の記述との実行結果が正しい場合はテストをパスします。

今回は`eq`という`RSpec`のマッチャと呼ばれるオブジェクトを使い、`Hello`と同一の値か調べています。

`to eq "Hello"` は `"Hello" と等しい ことを期待する`と言う意味になります。

`to`が`ことを期待する`、`eq`が`と等しい`と言い換えられます。

```rb
expect(get_hello).to eq "Hello"
```

つまり`get_helloの実行結果が"Hello"と等しいことを期待する`というテストケースを表しています。

一度テストを実行してみましょう。

`rspec`コマンドで`rspec`ディレクトリ以下がテストされます。

```rb
$ docker compose exec app bundle exec rspec
.

Finished in 0.00518 seconds (files took 0.08566 seconds to load)
1 example, 0 failures

```

最終行を見ると`example`が1、`failures`が0と、テストが成功していることがわかります。

```diff
  require 'spec_helper'
  require './hello'

  RSpec.describe 'Hello' do
    it "is equal Hello" do
      expect(get_hello).to eq "Hello"
    end
+
+   example "get_helloはHeyと一致しません。" do
+     expect(get_hello).not_to eq "Hey"
+   end
  end
```

`example ~ do`は`it ~ do`と全く同じメソッドです。書き方が異なるのみです。

日本では`example`を日本語でテストケースを書きたいときに使うことが多いです。

英語でテストケースを書きたいときに`it is ~`と英語の文章に近くなるため`it`を使用します。

今回は`Hey`と合致しないことをテストしたいので`to`ではなく、`not_to`という別のメソッドを使用しています。

つまり`get_helloの実行結果が"Hey"と等しいことを期待しない`というテストケースを表しています。

```sh
$ docker compose exec app bundle exec rspec
..

Finished in 0.00558 seconds (files took 0.08207 seconds to load)
2 examples, 0 failures
```

## minitest vs RSpec

`minitest`よりも`RSpec`のほうが圧倒的に優れているかといえばそうではありません。

`Ruby on Rails`の作者である`DHH`は`RSpec`が好きではないことで有名です。

https://twitter.com/dhh/status/647385298985447424

記法が嫌いなようです。

## おわりに

Fizz Buzzをコーディングするすべての準備が整いました。

次のチャプターでは実際に、
非常に難解と思われたFizz BuzzをRSpecの力を借りていともかんたんに解いていきます。
