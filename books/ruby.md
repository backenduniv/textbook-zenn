# Ruby

ここから、はじめてプログラミングについて学んでいきます。

本チャプターを学ぶことで今までのチャプターの理解も進むと思います。

ぜひ、本チャプターが終わったら、以前のチャプターを復習してみてください。

## プログラミング言語とは

プログラミング言語とはコンピュータプログラムを記述するための言語です。

言語の中でも形式言語と言われています。**形式言語**とは文法の曖昧さのない言語のことをいいます。

英語や、日本語のような文法が曖昧な言語のことを**自然言語**といいます。

そのため、英語のように訳者によって、解釈が異なるといったことが、プログラミング言語はありません。

## コンパイル

以前のチャンプターでも述べたとおり、コンピュータは直接理解できる0と1で記述されたバイナリしか実行できません。

このバイナリを**機械語**といいます。

機械語はコンピュータ上のCPUが直接実行していくものなので、CPUのためだけに設計されています。

人間が読み書きしやすいようには設計されていません。

昔は機械語を人間が直接入力していましたが、非常に大変な作業です。

そこで、**アセンブラ**という、機械語にほぼ1対1で対応するような言語が開発されました。 機械語よりも非常に人間が理解しやすいようなものです。

それでも、まだ書きにくいわけです。なので、より人間が理解しやすく、書きやすいプログラミング言語というものが開発されました。

このような経緯があるため、コンピュータでプログラミング言語で書かれたプログラムを実行するためには、下記のようなフローを辿る必要があります。

1. プログラミング言語をアセンブラに変換する
2. アセンブラを機械語に変換する

上記のプロセスを**コンパイル**といいます。**コンパイル**を実行するソフトウェアを**コンパイラ**といいます。

## プログラミング言語の種類

上記のようにプログラムを実行するためには、コンパイルを行なう必要がありますが、プログラミング言語によって、そのタイミングが異なります。

大きく分けて2つです。

1. 実行の前にコンパイルを行なう
2. 実行しながらコンパイルする

1の言語を**コンパイル言語**、2の言語を**インタプリタ言語**といいます。

正式な定義は存在しないのですが、理解がより進むために上記としました。

それぞれメリットがあります。デメリットはそれぞれのメリットの逆です。

- コンパイル言語
  - 実行の前に機械語に変換されるので実行が早い
  - 実行の前に文法的な誤りを検出できる
- インタプリタ言語
  - 実行時にコンパイルするで、一般的に時間のかかるコンパイルをせずに、すぐに実行できる
  - 一般的に記述量が少なく、可読性が高い

コンパイル言語と、インタプリタ言語の例を以下に示します。

- コンパイル言語
  - C
  - Java
  - Go
- インタプリタ言語
  - JavaScript
  - Ruby
  - Python

本チャプターではインタプリタ言語である、**Ruby**を学習します。

## Rubyとは

[Ruby](https://www.ruby-lang.org/ja/)

まつもとゆきひろ(通称: Matz)によって開発されたインタプリタ言語です。

Rubyは可読性を非常に重視した文法を採用しています。

そのため、初心者でも、学習しやすいプログラミング言語です。

Airbnbや、GitHubなどの世界中で使わているソフトウェアでも採用されている言語です。

## なぜRubyを学習するのか

初心者が学ぶ言語として、Rubyはベストなチョイスの一つと言えます。その理由は下記です。

1. 文法が非常にシンプル。
2. 日本語のドキュメントが豊富である
3. 他のプログラミング言語に文法が似ている
4. Ruby on Railsという強力なフレームワークがある

MatzはRubyの読みやすさ、書きやすさにこだわりがあります。

コンピュータが解釈すべきことは基本的に人間に書かせるのは間違っていると考えているため、無駄な記述を必要としません。

そのため、学習量も非常に少なく、動作するプログラムを記述できます。

そして、日本産プログラミング言語ということもあり、日本語のドキュメントが充実してます。

文法は、異なる点はあるとはいえ、有名な多言語と非常に似ています。

そのため、Rubyでプログラムを読み書きできるようになれば、ほとんどのプログラミング言語を簡単に習得できるようになります。

そして、RubyでWebアプリケーションを作成することを非常に容易にしてくれるRuby on Railsという**フレームワーク**があるのもRubyの魅力です。

アプリケーションの構築を楽にしてくれるプログラムによる枠組みのことです。

Ruby on RailsはWebアプリケーションフレームワークですので、Webアプリケーションの構築を楽にしてくれます。

Ruby on Railsはサーバーの機能や、データベースのデータの取得などのコードを直接書かなくても、すでに用意してくれています。

ソフトウェアエンジニアはフレームワーク上に肉付けをしていくことでアプリケーションを完成させることができます。

Rubyはこれほどまでにパワフルな言語なのです。

## セットアップ

それでは、手を動かしながら、実際にRubyを学んでいきましょう。

`~/code/burning-tech`に移動してください。

```sh
$ cd ~/code/burning-tech
```

教材リポジトリをクローンします。

```sh
$ git clone https://github.com/burning-tech/ruby-training.git
$ cd ruby-training
```

みなさん自身のリポジトリで管理してほしいので`.git`を削除して、`git init`しましょう。

```sh
$ rm -rf .git
$ git init
```

適宜remoteリポジトリをみなさんのGitHubアカウントに作成して、`git push`等してください。

続いて、Rubyプログラミングをしていくためにdockerを使用していきます。

```sh
# Dockerを起動していない場合は起動も必要です。
$ docker compose run --rm app
root@dc9982573086:/app#
```

このDockerコンテナにはRubyがすでにインストールされています。確認してみましょう。

`ruby --version`でバージョンが表示されていればOKです。

```sh
root@dc9982573086:/app# ruby -v
ruby 3.0.1p64 (2021-04-05 revision 0fb782ee38) [x86_64-linux]
```

本チャプターでは現在最新版であるバージョン`3.0.1`を使用します。

## Hello, world!(irb)

Rubyには2種類の実行方法があります。

1. ファイルを実行
2. 標準入力(キーボードからの入力)を実行する方法

アプリケーションの実行は1で行いますが、Rubyの学習や、挙動の確認には2は非常に便利です。

`irb`コマンドで開始できます。

irbとは**Interactive Ruby**の略です。

```sh
root@dc9982573086:/app# irb
irb(main):001:0>
```

プログラミングの世界には[Hello world](https://ja.wikipedia.org/wiki/Hello_world)というものがあります。

プログラミング言語の使用方法を理解するために`Hello world`という文字列を出力(画面に表示)することを指します。

やってみましょう。

```rb
irb(main):001:0> "Hello World"
=> "Hello World"
irb(main):002:0>
```

これだけでは何も理解できませんでしたね。ここからはRubyの詳細な構文(Syntax)を学んでいきます。

## 標準出力

以前、標準入力を紹介しました。今回は標準出力です。

標準入力とは入力もとを切り替えられるが、デフォルトではキーボードからの入力を指します。

標準出力の場合は出力先がデフォルトでは画面です。もちろん切り替えられます。

では、標準出力してみましょう。

```rb
irb(main):002:0> puts "a"
a
=> nil
```

`"a"`という文字列を出力しました。

`puts`がどういうものかは後ほど説明します。

わからなくなったら、何でもかんでも`puts`で出力してみてください。理解が進むはずです。

このように画面上に出力して、状況を把握したり、バグを発見する行為を**プリントデバッグ**といいます。

**デバッグ**とは**バグ**を発見するための作業のことです。

デバッグの実行を専業とするツールもありますが、もっともシンプルかつ、気軽にプログラムの動きを把握するために、頻繁にプリントデバッグをすること心がけましょう。

変だな？あれ？と思ったらすぐプリントデバッグです！

## 四則演算

まずは、四則演算をRubyで行ってみましょう。

### 加算

```rb
irb(main):003:0> 1 + 1
=> 2
```

### 減算

```rb
irb(main):004:0> 3 - 1
=> 2
```

### 乗算

```rb
irb(main):005:0> 4 * 0.5
=> 2.0
```

### 除算

```rb
irb(main):006:0> 4 / 2
=> 2
```

乗算は×ではなく、`*`で行います。

徐算は÷ではなく、`/`で行います。

`+`、`-`は同じです。

簡単ですね。

`+`、`-`、`*`、`/`のことをプログラミングにおいて、**演算子**(**Operator**)といいます。

これらの演算子の対象となる値(`3 - 1`の`3`と`1`)のことを**被演算子**(**Operand**)といいます。

覚えておいてください。

下記のように演算を結合できます。

```rb
irb(main):007:0> (5 - 1) / 2 * 10 -18
=> 2
```

`()`は小学生が算数で習う`()`と同じルールと全く同じです。

一つ違うとすれば`()`は一つしかなく、`{}`、`[]`は使えません。

下記のように`()`は何度使用しても構いません。

```rb
irb(main):008:0> ((5 - 1) - 2) * 2
=> 4
```

加えてあと一つ触れておくと、`4 * 0.5`の結果だけ`2.0`と小数点以下が表示されました。

これは以前Relational Databaseのチャプターで触れた`データ型`と関係があります。

頭の片隅に置いておいてください。

## 変数

四則演算を組み合わせより複雑な演算をしてみましょう。

小学生の頃に戻って、円柱の体積を求めてみましょう。

```
円柱の体積 = 底面積(半径×半径×円周率) × 高さ
```

半径が5cm、高さが10cm、円周率を3.14とします。

すると下記のようになりますね。

```rb
irb(main):009:0> 5 * 5 * 3.14 * 10
=> 785.0
```

しかし、これだけ見て、すぐに円柱の体積を求めているとわかるでしょうか？

仮に、5000cm3に空間に5つの円柱が存在し、円柱が占めていない空間の体積を求める場合はどうでしょうか？

```rb
irb(main):012:0> 5000 - 5 * 5 * 3.14 * 10 * 5
=> 1075.0
```

ここから意味を読み取るのはほぼ不可能です。

半径と円柱の個数が同じ5でどれがどれを表すのがめちゃくちゃです。

ここで**変数**というものが有用になってきます。

変数とは**ある値に名前をつけたもの**です。

実際に変数を作成してみましょう。

```rb
irb(main):013:0> x = 1
=> ...
```

これは`1`という値に`x`という名前をつけた変数です。

値に名前をつけることを**代入**といいます。

新しく変数を作成することを変数を**宣言**するといいます。

つまり、`x = 1`は**代入によって、変数を宣言した**ということになります。

```rb
irb(main):014:0> x
=> 1
```

`x`が`1`に名前をつけたものであることがわかります。

では、円柱の体積を求める式に変数を使用してみましょう。

```rb
irb(main):015:0> radius = 5
=> ...
irb(main):016:0> pi = 3.14
=> ...
irb(main):017:0> height = 10
=> ...
irb(main):018:0> radius * radius * pi * height
=> 785.0
```

非常にわかりやすくなりました！

```rb
irb(main):019:0> space = 5000
=> ...
irb(main):020:0> count = 5
=> ...
irb(main):021:0> space - radius * radius * pi * height * count
=> 1075.0
```

ちなみにRubyで累乗の演算子は`**`です。

下記のように円柱の体積の式を変更できます。

```rb
irb(main):022:0> radius ** 2 * pi * height
=> 785.0
```

では、円柱の体積の計算結果を変数に代入してみましょう。

```rb
irb(main):023:0> cylinder_volume = radius ** 2 * pi * height
=> ...
irb(main):025:0> space - cylinder_volume * count
=> 1075.0
```

変数がいかに便利なものか理解していただけたでしょうか？

ちなみにRubyは変数名は小文字で、`_`を単語同士の区切り記号として使用する**スネークケース**という記法を使用してください。

```rb
irb(main):026:0> aaaBBB = 1
=> ...
irb(main):027:0> aaaBBB
=> 1
```

上記のように`_`区切りでなく、小文字でもない変数も使用できますが、開発者の中ではスネークケースを使用する決まりになっています。

このような決まりごとを**命名規則**といいます。

Airbnbは[Ruby Style Guide](https://github.com/airbnb/ruby)というのを公開しています。

このような規則を開発者同士で遵守することで保守性の高いソースコード管理が可能となります。

Ruby Style Guideは非常に学びが多いですので、定期的に読んで、これを守ることをおすすめします。

変数には別の値を代入できます。

下記では、もともと`10`に対して`height`という名前で変数を代入していましたが、`15`を代入して変更しています。

```rb
irb(main):028:0> height = 15
=> ...
irb(main):029:0> radius ** 2 * pi * height
=> 1177.5
```

## メソッド

上記では何度も、円柱の体積を計算しました。

同じコードを何度も書いています。

ソフトウェアの世界には[Don't repeat yourself](https://xn--97-273ae6a4irb6e2hsoiozc2g4b8082p.com/%E3%82%A8%E3%83%83%E3%82%BB%E3%82%A4/DRY%E5%8E%9F%E5%89%87/)(DRY)という考え方があります。

重複をなるべく排除し、保守性の高いソースコードにしようという考え方です。

つまり何度も`radius ** 2 * pi * height`と書くのはDRY原則に違反しているわけです。

では重複を排除できるようにしましょう。

そのためには、**メソッド**(Method)を使用します。

メソッドとは、一連の処理をひとかたまりに切り分けて名前をつけ、他の場所からでも呼び出せるようにしたものです。

まずは非常にかんたんなケースから学んでいきましょう。

シンタックスは下記のとおりです。

```rb
def メソッド名
  実行する処理
end
```

メソッドを作成することをメソッドを**定義**(Definition)するといいます。(そのため、`def`を使用します。)

では`Hello`と標準出力に出力するメソッドを定義してみましょう。

```rb
irb(main):030:1* def print_hello
irb(main):031:1*   puts "Hello"
irb(main):032:0> end
=> :print_hello
```

実行してみましょう。実行はメソッド名を入力するだけで、できます。

```rb
irb(main):033:0> print_hello
Hello
=> nil
```

`Hello`と出力されています！

何度だって呼び出せます。

```rb
irb(main):035:0> print_hello
Hello
=> nil
irb(main):035:0> print_hello
Hello
=> nil
```

では、もとの円柱の体積を求めるコードをメソッドにしてみましょう。

下記の様に定義しましょう。

```rb
def calculate_cylinder_volume
  radius = 5
  pi = 3.14
  height = 10
  puts radius ** 2 * pi * height
end
```

```rb
irb(main):036:1* def calculate_cylinder_volume
irb(main):037:1*   radius = 5
irb(main):038:1*   pi = 3.14
irb(main):039:1*   height = 10
irb(main):040:1*   puts radius ** 2 * pi * height
irb(main):041:0> end
=> :calculate_cylinder_volume
```

```sh
irb(main):042:0> calculate_cylinder_volume
785.0
=> nil
```

円柱の体積を何度だって計算できるようになりました。

### 引数

ですが、このままでは、半径5cm、高さ10cmの円柱の体積しか求められません。

そんなときのために、メソッドは実行の際に半径や、高さなどを設定できる機能があります。

これを**引数**(Argument)といいます。

```rb
def メソッド名(引数)
  引数を使った処理
end
```

引数は複数の設定できます。

```rb
def メソッド名(引数、引数...)
  引数を使った処理
end
```

実際に使ってみましょう。

メソッド名の後に`()`を書き、その間に引数を書きます。

`()`はなくても動きますが、引数があるメソッドで`()`を書かないと可読性が非常に下がるのでやめましょう。

引数がない場合には`()`が不要になると考えてください。

```rb
def calculate_cylinder_volume(radius, height)
  pi = 3.14
  puts radius ** 2 * pi * height
end
```

```rb
irb(main):043:1* def calculate_cylinder_volume(radius, height)
irb(main):044:1*   pi = 3.14
irb(main):045:1*   puts radius ** 2 * pi * height
irb(main):046:0> end
=> :calculate_cylinder_volume
```

メソッドの定義時にはまだ、`radius`と`height`引数は値が設定されていません。

メソッドの実行の際に、`radius`と`height`に値が代入される変数だと考えてください。

つまり、メソッド定義時には変数宣言だけ行って、値の代入していないのが、引数ということになります。

引数のあるメソッドの実行は下記の通りです。

```rb
メソッド名(引数として渡したい値)
```

実行してみましょう。

```rb
=> :calculate_cylinder_volume
irb(main):047:0> calculate_cylinder_volume(5, 10)
785.0
=> nil
irb(main):049:0> calculate_cylinder_volume(3, 4)
113.04
=> nil
```

別の半径、高さを渡してもまさしく計算されています！

下記のように変数を渡してもメソッドを実行できます。

```rb
irb(main):050:0> r = 5
=> ...
irb(main):051:0> h = 10
=> ...
irb(main):053:0> calculate_cylinder_volume(r, h)
785.0
=> nil
```

実行時に実際に渡す引数を**実引数**(Argument)、メソッドの定義時に使用されている引数を**仮引数**(Parameter)といいます。

メソッドに渡される値がどんなものなのかは、詳細に理解する必要があります。

ですが、今の知識量では、詳細な理解は遠回りになりますので、現状は下記のように理解してください。

```
メソッドに実引数として値を渡す => メソッド内の仮引数に値が代入される
```

### 返り値

さきほど例に出した5000cm3に空間に5つの円柱が存在し、円柱が占めていない空間の体積を求めてみましょう。

もともとは下記のようなコードを書いていました。

```rb
space - radius * radius * pi * height * count
```

これを下記のように記述できれば、非常に読みやすいコードなると思いませんでしょうか？

```rb
space - calculate_cylinder_volume(5, 10) * count
```

ですが、現状はこれはできません。

```rb
irb(main):020:0> space - calculate_cylinder_volume(5, 10) * count
785.0
(irb):20:in `<main>': undefined method `*' for nil:NilClass (NoMethodError)
        from /usr/local/lib/ruby/gems/3.0.0/gems/irb-1.3.5/exe/irb:11:in `<top (required)>'
        from /usr/local/bin/irb:23:in `load'
        from /usr/local/bin/irb:23:in `<main>'
```

色々と、多くの出力がされました。

`NoMethodError`と表示されています。エラーです！

`785.0`が標準出力に出力されたあとに、エラーが起きていることを意味します。

`calculate_cylinder_volume(5, 10)`で計算した値を標準出力に出力するのではなく、メソッドの外に返せるようにする必要があります。

そのためには`return`をメソッド内で使用します。

```rb
def メソッド名
  処理
  return 処理で計算した値
end
```

もちろん引数とも一緒に使えます。

```rb
def メソッド名(引数、引数...)
  引数を使った処理
  return 処理で計算した値
end
```

再度、`calculate_cylinder_volume`を`return`を使用して書き直してみましょう。

```rb
def calculate_cylinder_volume(radius, height)
  pi = 3.14
  return radius ** 2 * pi * height
end
```

```rb
irb(main):021:1* def calculate_cylinder_volume(radius, height)
irb(main):022:1*   pi = 3.14
irb(main):023:1*   return radius ** 2 * pi * height
irb(main):024:0> end
=> :calculate_cylinder_volume
```

では使用してみましょう。

```rb
irb(main):025:0> space - calculate_cylinder_volume(5, 10) * count
=> 1075.0
```

`return`を使用して、メソッドから返却される値を**返り値**といいます。

`return`をメソッド内で使用すると、メソッドの処理はそこで終了し、それ以降の処理は実行されません。

下記の例では、`"hi"`と、`hey`は出力されません。

```rb
irb(main):027:1* def func
irb(main):028:1*   return 1
irb(main):029:1*   puts "hi"
irb(main):030:1*   puts "hey"
irb(main):031:0> end
=> :func
irb(main):032:0> func
=> 1
```

メソッドの最後に`return`を書く場合は、`return`を省略できます。

```rb
def calculate_cylinder_volume(radius, height)
  pi = 3.14
  radius ** 2 * pi * height
end
```

```rb
irb(main):033:1* def calculate_cylinder_volume(radius, height)
irb(main):034:1*   pi = 3.14
irb(main):035:1*   radius ** 2 * pi * height
irb(main):036:0> end
=> :calculate_cylinder_volume
irb(main):037:0> calculate_cylinder_volume(5, 10)
=> 785.0
```

`return`を記述したときと同じように`785.0`が返却されています。

メソッドの最終行でない場合は、`return`を省略できません。

```rb
irb(main):045:1* def func
irb(main):046:1*   1
irb(main):047:1*   puts "hi"
irb(main):048:1*   puts "hey"
irb(main):049:0> end
=> :func
irb(main):050:0> func
hi
hey
=> nil
```

## 条件分岐

上記で計算したケースを膨らませて考えてみましょう。

```
円柱の体積がxより大きいかどうかを調べる
```

`x`の値を`5000`としてやってみます。

```rb
irb(main):059:0> x - calculate_cylinder_volume(5, 10)
=> 4215.0
```
このように私達は今までの知識で円柱の体積がXより多いかどうかを調べることができます。

しかし、それは計算の結果が0より大きいかどうかを私達が判断している状態です。

5つの円柱の体積がXより大きいときには`Big!`と表示するプログラムに変更しましょう。

このためには`if`を使用します。

### if

```rb
if 条件式 then
  処理
end
```

`if`と`then`の間に条件式を記述します。

条件式が評価され、成立した場合`then`以下に記述された処理を実行します。

具体的なコードを見てみましょう。

```rb
result = x - calculate_cylinder_volume(5, 10)
if result > 0 then
  puts "Big!"
end
```

```rb
irb(main):060:0> result = x - calculate_cylinder_volume(5, 10)
=> ...
irb(main):061:1* if result > 0 then
irb(main):062:1*   puts "Big!"
irb(main):063:0> end
Big!
=> nil
```

`result`という変数に計算の結果を代入し、`result`が0より大きいときに`Big!`と出力しています。

ここで出てきた`>`は**関係演算子**というものです。

上記であれば、`0`より`result`のほうが大きければ、処理を実行するという条件式です。

関係演算子の説明の前に**条件式**について、詳細な説明をする必要があります。

条件式に記述された式が返却する値が**真**か**偽**かというのを判定しています。

- 偽 => `false`、`nil`
- 真 => `false`、`nil`以外の値

`nil`とは何も無いということを示すための特殊な変数です。

`nil`の説明はもっと先で行なうので、`false`を説明しましょう。

`if`の条件式では、`AがBより大きい`、`AとBが等しい`などが記述されます。

条件式の真偽を判定することを**評価**するといいます。

この式が評価され結果として条件が成立する場合と成立しない場合に分岐します。

条件が評価され、成立した場合は条件式は`true`を返却します。

一方、条件が評価され、不成立だった場合は`false`を返却します。

`result > 0`だけを実行してみましょう。

```rb
irb(main):074:0> result > 0
=> true
```

一方、`result < 0`は`false`を返却します。

```rb
irb(main):075:0> result < 0
=> false
```

`true`、`false`(、`nil`)のような真偽を判定する値を**真偽値**といいます。

先に説明したように、`false`、`nil`以外の値はすべて真ですのでこんな事もできます。

```rb
irb(main):076:1* if 1 then
irb(main):077:1*   puts "hi"
irb(main):078:0> end
hi
=> nil
```

では、関係演算子について説明を戻しましょう。

関係演算子とは演算の結果が`true`もしくは`false`を返却する演算子です。

| 演算子 | 記述例 | 意味                               | 例       | 結果    |
| -      | -      | -                                  | -        | -       |
| >      | A > B  | AがBより大きい                     | `2 > 1`  | `true`  |
| >=     | A >= B | AがBより大きい、またはAとBが等しい | `2 >= 1` | `true`  |
| <      | A < B  | AがBより小さい                     | `2 < 1`  | `false` |
| <=     | A <= B | AがBより小さい、またはAとBが等しい | `2 <= 1` | `true`  |
| ==     | A == B | AがBと等しい                       | `2 == 1` | `false` |
| !=     | A != B | AがBと等しくない                   | `2 != 1` | `true`  |

再度、下記を説明すると`result > 0`が`true`を返却し、`true`は真と評価されるので`Big!`が表示されたということになります。

```rb
result = x - calculate_cylinder_volume(5, 10)
if result > 0 then
  puts "Big!"
end
```

### else

`if`には条件式が偽と評価された場合にも処理を実行できます。

```rb
if 条件式 then
  真のときの処理
else
  偽のときの処理
end
```

下記のようなコードを実行してみましょう。

```rb
                                     # 20に変更しています。
result = x - calculate_cylinder_volume(20, 10)
if result > 0 then
  puts "Big!"
else
  puts "Small!"
end
```

```rb
irb(main):085:0> result = x - calculate_cylinder_volume(20, 10)
=> ...
irb(main):086:1* if result > 0 then
irb(main):087:1*   puts "Big!"
irb(main):088:1* else
irb(main):089:1*   puts "Small!"
irb(main):090:0> end
Small!
```

`Big!`とは表示されず、`Small!`だけが表示されました。

### elsif

条件式を複数用意したい場合は`elsif`を使用します。

```rb
                                               # 785.0を引いて0にしています。
result = x - calculate_cylinder_volume(5, 10) - 4215.0
if result > 0 then
  puts "Big!"
elsif result == 0
  puts "Zero!"
else
  puts "Small!"
end
```

```rb
irb(main):121:0> result = x - calculate_cylinder_volume(5, 10) - 4215.0
=> ...
irb(main):122:1* if result > 0 then
irb(main):123:1*   puts "Big!"
irb(main):124:1* elsif result == 0
irb(main):125:1*   puts "Zero!"
irb(main):126:1* else
irb(main):127:1*   puts "Small!"
irb(main):128:0> end
Zero!
=> nil
```

### thenの省略

真のときの処理が次の行以降に記述されている場合、`then`を省略できます。


```rb
result = x - calculate_cylinder_volume(5, 10)
if result > 0
  puts "Big!"
else
  puts "Small!"
end
```

```rb
irb(main):091:0> result = x - calculate_cylinder_volume(5, 10)
=> ...
irb(main):092:1* if result > 0
irb(main):093:1*   puts "Big!"
irb(main):094:1* else
irb(main):095:1*   puts "Small!"
irb(main):096:0> end
Big!
=> nil
```

### unless

偽の処理を先に記述したい場合は`unless`を使用します。

つまり`if`の逆になります。

```rb
result = x - calculate_cylinder_volume(5, 10)
unless result > 0
  puts "Small!"
else
  puts "Big!"
end
```

```rb
irb(main):097:0> result = x - calculate_cylinder_volume(5, 10)
=> ...
irb(main):098:1* unless result > 0
irb(main):099:1*   puts "Small!"
irb(main):100:1* else
irb(main):101:1*   puts "Big!"
irb(main):102:0> end
Big!
=> nil
```

### case

`elsif`はいくつもつけることができます。

```rb
color = 'red'
if color == "red"
  puts "赤"
elsif color == "blue"
  puts "青"
elsif color == "green"
  puts "緑"
end
```

```rb
irb(main):129:0> color = 'red'
=> ...
irb(main):130:1* if color == "red"
irb(main):131:1*   puts "赤"
irb(main):132:1* elsif color == "blue"
irb(main):133:1*   puts "青"
irb(main):134:1* elsif color == "green"
irb(main):135:1*   puts "緑"
irb(main):136:0> end
赤
```

しかしこのように`elesif`を使用するより`case`のほうがスッキリかけて便利です。

```rb
color = 'red'
case color
when "red"
  puts "赤"
when "blue"
  puts "青"
when "green"
  puts "緑"
end
```

```rb
irb(main):137:0> color = 'red'
=> ...
irb(main):138:1* case color
irb(main):139:1* when "red"
irb(main):140:1*   puts "赤"
irb(main):141:1* when "blue"
irb(main):142:1*   puts "青"
irb(main):143:1* when "green"
irb(main):144:1*   puts "緑"
irb(main):145:0> end
赤
```

### 三項演算子

短い`if`を使う処理の場合は、**三項演算子**を使用することで、きれいにかけることがあります。

```rb
条件式 ? 真のときの処理 : 偽のときの処理
```

```rb
irb(main):159:0> result = x - calculate_cylinder_volume(5, 10)
irb(main):160:0> puts result > 0 ? "Big!" : "Small!"
Big!
=> nil
```

上記は`puts (result > 0 ? "Big!" : "Small!")`とするとわかりやすいと思います。

## リテラル

今まで、電卓レベルの計算ばかりをやってきました。もっと複雑なことをやるためには数字以外を扱えるようになる必要があります。


まずは、ソースコード上に直接記述できる値である**リテラル**を学びましょう。

リテラルはさまざま種類がありますが、ここでは、3種類を学びます。

### 数値リテラル

数値(Number)リテラルは整数、浮動小数点数、複素数、進数とあらゆる数値を取り扱えます。

`1`や、`1.0`のように記述することで使用できます。

今まで取り扱ってきたリテラルです。

数値リテラルで使用できる、まだ紹介していない`%`を紹介します。

`%`を使用して、余りを求めることができます。

```rb
irb(main):209:0> 5 % 2
=> 1
```

#### 自己代入

下記の処理は今までと同じような処理を学んできたので理解できるはずです。

```rb
a = 1
b = 2
c = a + b
puts c
```

```rb
irb(main):211:0> a = 1
=> ...
irb(main):212:0> b = 2
=> ...
irb(main):213:0> c = a + b
=> ...
irb(main):214:0> puts c
3
=> nil
```

では、下記はどうなるでしょうか？

```rb
a = 1
b = 2
a = a + b
puts a
```

```rb
irb(main):215:0> a = 1
=> ...
irb(main):216:0> b = 2
=> ...
irb(main):217:0> a = a + b
=> ...
irb(main):218:0> puts a
3
=> nil
```

上記は一度`a + b`が`3`であると計算され、`a`に代入されていることを表しています。

上記の`a = a + b`を`+=`を使って短く書くことができます。全く同じ意味です。

```rb
a = 1
b = 2
a += b
puts a
```

```rb
irb(main):224:0> a = 1
=> ...
irb(main):225:0> b = 2
=> ...
irb(main):226:0> a += b
=> ...
irb(main):227:0> puts a
3
=> nil
```

これを**自己代入**といいます。

### 文字列リテラル

文字列(String)はダブルクォート(`"`)またはシングルクォート(`'`)で囲まれたリテラルです。

```rb
irb(main):180:0> a = "a"
=> ...
irb(main):181:0> b = 'b'
=> ...
irb(main):182:0> a
=> "a"
irb(main):183:0> b
=> "b"
```

`"`の場合は改行コード(`\n`)を入力すると改行させることができます。

```rb
irb(main):201:0> puts "aaa\nbbb"
aaa
bbb
=> nil
```

`'`の場合は改行されません。

```rb
irb(main):202:0> puts 'aaa\nbbb'
aaa\nbbb
=> nil
```

文字列も数値と同様に`+`を使用して結合することがきます。

```rb
irb(main):203:0> 'aaa' + 'bbb'
=> "aaabbb"
```

`"`では`#{}`を使って、変数の値を展開できます。

(`'`は展開されません。)

```rb
irb(main):205:0> triple_a = "aaa"
=> ...
irb(main):206:0> "bb#{triple_a}bb"
=> "bbaaabb"
```

文字列リテラルも`==`、`!=`を使って比較できます。

```rb
irb(main):207:0> "aaa" == "aaa"
=> true
irb(main):208:0> "aaa" != "bbb"
=> true
```

### 配列リテラル

今まで一つのデータは一つの変数に代入して取り扱ってきました。

ここでは、複数のデータを一つの変数に代入してまとめて使用できる**配列**(Array)リテラルについて学びます。

配列は`[]`を使用して作成します。

```rb
irb(main):228:0> []
=> []
```

これは空の配列を意味します。

では要素を持った配列を用意しましょう。

```rb
irb(main):229:0> [1, 2, 3]
=> [1, 2, 3]
irb(main):233:0> ["hoge", "huga", "piyo"]
=> ["hoge", "huga", "piyo"]
irb(main):234:0> [1, "hoge", 1.2]
=> [1, "hoge", 1.2]
```

上記のように、文字列でも、数値でも、それらが一緒でも取り扱ってうことができます。

```rb
irb(main):235:0> [[1, 2, 3], [4, 5, 6]]
=> [[1, 2, 3], [4, 5, 6]]
```

`[]`を重ねることで複数次元の配列を作成できます。

では、配列の中の値を取得してみましょう。

```rb
irb(main):236:0> a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
=> ...
```

`a[何番目]`というふうにアクセスします。

```rb
irb(main):237:0> a[0]
=> 1
```

この何番目という添字を**インデックス**(Index)といいます。

配列の各要素には，先頭を0番として，0, 1, 2, 3と，順に番号がついています。

先頭が0番のため，要素の数がnの場合には配列の最後の要素はn-1番です。

配列の長さは`配列.size`で取得できます。

```rb
irb(main):238:0> a.size
=> 9
```

そのため、最後の要素は下記のように取得できます。

```rb
irb(main):241:0> a[a.size - 1]
=> 9
```

実は`-1`でも最後の要素にアクセスできます。

```rb
irb(main):242:0> a[-1]
=> 9
```

要素を変更してみましょう。

インデックスを指定して、代入することで変更できます。

```rb
irb(main):243:0> a[4] = 300
=> 300
```

```rb
irb(main):244:0> a
=> [1, 2, 3, 4, 300, 6, 7, 8, 9]
```

インデックスが4の値が変更できました。

配列に新しい値を追加するには`<<`を使用します。

```rb
irb(main):245:0> a << 1000
=> [1, 2, 3, 4, 300, 6, 7, 8, 9, 1000]
```

## 繰り返し処理

数値配列内の要素の合計を計算してみましょう。

```rb
irb(main):284:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):285:0> sum = 0
=> ...
irb(main):286:0> sum +=a[0]
=> ...
irb(main):287:0> sum +=a[1]
=> ...
irb(main):288:0> sum +=a[2]
=> ...
irb(main):289:0> sum +=a[3]
=> ...
irb(main):290:0> sum +=a[4]
=> ...
irb(main):291:0> sum
=> 15
```

この処理には2つの問題点があります。

1. 同じ処理が何度も重複している
2. 違う長さの配列に対応できない。
3. より長い配列だとつらい

何度も同じ処理繰り返し実行してくれる機能がRubyにはあります。

### each

`each`を使用することで配列の要素のすべてに対して同じ処理を実行できます。

下記のように記述します。

```rb
配列.each do |配列の要素が渡される引数|
  処理
end
```

eachを使用して、配列の要素を合計してみましょう。

```rb
a = [1, 2, 3, 4, 5]
sum = 0
a.each do |n|
  sum += n
end
sum
```

`a`の要素が取り出され、`n`に渡されます。そして`end`までの処理が実行されます。
そして、処理が終わると次の`a`の要素が取り出され、`n`に渡されます。

```rb
irb(main):292:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):293:0> sum = 0
=> ...
irb(main):294:1* a.each do |n|
irb(main):295:1*   sum += n
irb(main):296:0> end
=> [1, 2, 3, 4, 5]
irb(main):297:0> sum
=> 15
```

非常にスマートに、そして短く書くことができました。

同じことをする別の書き方もあります。

```rb
配列.each { |配列の要素が渡される引数|
  処理
}
```

これは`do`を使う処理とは異なり、一行で書くことができます。

```rb
配列.each { |配列の要素が渡される引数| 処理 }
```

```rb
irb(main):298:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):299:0> sum = 0
=> ...
irb(main):300:0> a.each { |n| sum += n }
=> [1, 2, 3, 4, 5]
irb(main):301:0> sum
=> 15
```

一行でかけるときは`{}`を使い、複数行書く必要がある場合は`do`を使用しましょう。

`|配列の要素が渡される引数|`のことを**ブロック引数**といいます。

ブロックとは`each`で使用している`do`から`end`まで、または`{}`の中のことです。

ブロック引数はブロックの中だけで使用できます。

### map

繰り返しの処理は`each`だけではありません。

`map`は配列から新たな配列を作成できます。

```rb
新しい配列 = 配列.map { |配列の要素が渡される引数|
  処理
}
```

ここでは`{}`を使用して、記述していますが、以降の繰り返しの処理では共通して`do`を使用できます。

配列の要素をそれぞれ2倍にした新しい配列を作成してみましょう。

```rb
a = [1, 2, 3, 4, 5]
new_a = a.map { |n| n * 2 }
new_a
```

```rb
irb(main):305:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):306:0> new_a = a.map {|n| n * 2}
irb(main):307:0> new_a
=> [2, 4, 6, 8, 10]
```

### select

`select`は配列のそれぞれの要素に対して評価をし、真の要素だけをまとめた配列を作成できます。

```rb
新しい配列 = 配列.select { |配列の要素が渡される引数| 条件式 }
```

配列から要素が3以上の値のみの新たな配列を作成してみましょう。

```rb
a = [1, 2, 3, 4, 5]
new_a = a.select { |n| n >= 3 }
new_a
```

```rb
irb(main):313:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):314:0> new_a = a.select { |n| n >= 3 }
=> ...
irb(main):315:0> new_a
=> [3, 4, 5]
```

その逆の偽の値のみをまとめる`reject`もあります。

下記の場合、3以上の場合は除外されます。

```rb
a = [1, 2, 3, 4, 5]
new_a = a.reject { |n| n >= 3 }
new_a
```

```rb
irb(main):316:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):317:0> new_a = a.reject { |n| n >= 3 }
=> ...
irb(main):318:0> new_a
=> [1, 2]
```

### find

`find`は条件に合致した最初の要素を返却できます。

```rb
条件に合致した最初の要素 = 配列.find { |配列の要素が渡される引数| 条件式 }
```

下記は3以上という条件に合致した最初の要素として、3が返却されています。

```rb
a = [1, 2, 3, 4, 5]
n = a.find { |n| n >= 3 }
n
```

```rb
irb(main):319:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):320:0> n = a.find { |n| n >= 3 }
=> ...
irb(main):321:0> n
=> 3
```

### inject

`inject`を使うと最初に`map`を使用していった、配列の要素をすべて足し合わせるという処理をより短く書くことができます。

下記がもともとの`map`を使用したコードです。

```rb
a = [1, 2, 3, 4, 5]
sum = 0
a.map {|n| sum += n}
sum
```

`inject`を使用すると下記のようになります。

```rb
a = [1, 2, 3, 4, 5]
sum = a.inject(0) {|result, n| result + n }
sum
```

```rb
irb(main):329:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):330:0> sum = a.inject(0) {|result, n| result + n }
=> ...
irb(main):331:0> sum
=> 15
```

文法は下記の通りです。

```rb
計算結果 = 配列.inject(初期値) { |最終的に返却される値, 配列の要素が渡される引数| 処理 }
```

処理を追っていきましょう。

1. 一巡目

まず、injectの引数(`()`の中)は最終的に返却される値の初期値です。なので、例では`result`に`0`が代入されます。

そして、`n`にはいつものように配列`a`の最初の要素(`1`)が渡されます。

ブロックの中で`reslut + n`つまり`0 + 1`の計算結果(`1`)が`result`に代入されます。一巡終了です。

2. 二巡目

`result`には前回の計算結果として`1`が入っています。そして、`n`には配列の2番目の要素(`2`)が入っています。

ブロックの中で`reslut + n`つまり`1 + 2`の計算結果(`3`)が`result`に代入されます。二巡目終了です。

3. 三巡目

`result`には前回の計算結果として`3`が入っています。そして、`n`には配列の3番目の要素(`3`)が入っています。

ブロックの中で`reslut + n`つまり`3 + 3`の計算結果(`6`)が`result`に代入されます。三巡目終了です。

...というように続いていき、すべての要素の処理が終了した段階で、`result`の値(`15`)が返却されます。

### loop

配列を操作するのから離れて、無限ループを使う方法`loop`について触れます。

```rb
loop do
  処理
end
```

このように記述することで無限に処理を繰り返します。

```rb
loop do
  puts "hoge"
end
```

```rb
irb(main):344:1* loop do
irb(main):345:1*   puts "hoge"
irb(main):346:1* end
hoge
hoge
hoge
hoge
hoge
hoge
hoge
hoge
...
```

終了するには`Ctrl`+`C`を入力してください。

無限ループから抜けるには`break`を使用できます。

下記は`sum`が`10`以上になったら、`break`で繰り返しの処理を抜けます。

```rb
sum = 0
loop do
  sum +=1
  puts "hoge"
  if sum > 10
    break
  end
end
```

```rb
irb(main):363:0> sum = 0
=> ...
irb(main):364:1* loop do
irb(main):365:1*   sum +=1
irb(main):366:1*   puts "hoge"
irb(main):367:2*   if sum > 10
irb(main):368:2*     break
irb(main):369:1*   end
irb(main):370:0> end
hoge
hoge
hoge
hoge
hoge
hoge
hoge
hoge
hoge
hoge
hoge
=> nil
```

## 制御構文

前で説明した`break`は**制御構文**といわれるものです。

制御構文は3つあります。

- break
- next
- redo

上記は`each`などの配列の繰り返しにも使用できます。

下記は`10`以上で繰り返しを抜けるので、最後の要素`5`が足されず、終了しています。

```rb
a = [1, 2, 3, 4, 5]
sum = 0
a.each do |n|
  sum += n
  if sum >= 10
    break
  end
end

sum
```

```rb
irb(main):371:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):372:0> sum = 0
=> ...
irb(main):373:1* a.each do |n|
irb(main):374:1*   sum += n
irb(main):375:2*   if sum >= 10
irb(main):376:2*     break
irb(main):377:1*   end
irb(main):378:0> end
=> nil
irb(main):379:0>
irb(main):380:0> sum
=> 10
```

### next

`next`は`break`とは異なり、繰り返しを抜けるのではなく、次の繰り返し処理にスキップします。

下記は要素が5より小さいときは次のループ処理にスキップするので、最後の要素のみが`sum`に加算されます。

```rb
a = [1, 2, 3, 4, 5]
sum = 0
a.each do |n|
  if n < 5
    next
  end
  sum += n
end

sum
```

```rb
irb(main):381:0> a = [1, 2, 3, 4, 5]
=> ...
irb(main):382:0> sum = 0
=> ...
irb(main):383:1* a.each do |n|
irb(main):384:2*   if n < 5
irb(main):385:2*     next
irb(main):386:1*   end
irb(main):387:1*   sum += n
irb(main):388:0> end
=> [1, 2, 3, 4, 5]
irb(main):389:0>
irb(main):390:0> sum
=> 5
```

## ハッシュリテラル

ハッシュはリテラルのひとつです。

複数のデータを一つの変数に代入してまとめて使用できるものが配列でした。

ハッシュは**キー**と**バリュー**といわれる２つの要素のペアをまとめて保存できるものです。

実際に触って、どんなものなのか学んでいきましょう。

`{}`を使用して、ハッシュリテラルを記述します。

これは空のハッシュです。

```rb
irb(main):391:0> {}
=> {}
```

キーとバリューは下記のように記述します。

```rb
{キーその1 => バリューその1, キーその2 => バリューその2...}
```

キーを英語、バリューを日本語の対応表をハッシュにすると下のようになると思います。

```rb
colors = {'red' => '赤', 'blue' => '青', 'green' => '緑'}
colors
```

```rb
irb(main):393:0> colors = {'red' => '赤', 'blue' => '青', 'green' => '緑'}
=> ...
irb(main):394:0> colors
=> {"red"=>"赤", "blue"=>"青", "green"=>"緑"}
```

ハッシュの値にはキーを指定することアクセスできます。

```rb
colors["red"]
```

```rb
irb(main):395:0> colors["red"]
=> "赤"
```

キーによるアクセスは配列から`select`などで検索するより非常に高速です。

ハッシュにキーとバリューのペアを追加する際は下記のように記述します。

```rb
colors["white"] = "白"
```

```rb
irb(main):396:0> colors["white"] = "白"
irb(main):399:0> colors
=> {"red"=>"赤", "blue"=>"青", "green"=>"緑", "white"=>"白"}
```

存在するキーを指定すると、もとの値が上書きされます。

```rb
irb(main):401:0> colors["red"] = "あか"
=> ...
irb(main):402:0> colors
=> {"red"=>"あか", "blue"=>"青", "green"=>"緑", "white"=>"白"}
```

ハッシュにも`each`などの繰り返し処理が実行できます。

```rb
ハッシュ.each do |ハッシュのキーが渡される引数, ハッシュのバリューが渡される引数|
  処理
end
```

```rb
colors.each { |key, value| puts "#{key}は日本語で#{value}"}
```

```rb
irb(main):403:0> colors.each { |key, value| puts "#{key}は日本語で#{value}"}
redは日本語であか
blueは日本語で青
greenは日本語で緑
whiteは日本語で白
=> {"red"=>"あか", "blue"=>"青", "green"=>"緑", "white"=>"白"}
```

## クラス

ここまでで紹介した機能で、かなり幅広いプログラムを書けるようになりました。

ここからは、Rubyの核となる機能について解説します。

今までこれはこういうものなんだと飲み込んできたコードが、「だからこうなんだ！」と、理解できるようになるはずです！

### クラスとは

リテラルについてはすでに学びました。数値リテラル、文字列リテラル、配列リテラル、ハッシュリテラルの4つです。

`0`、`"hoge"`、`[]`、`{}`でしたね？

これらは、リテラルごとに、できることが異なっていました。

足し算をしたり、文章を組み上げたり、同じ処理を何度も実行したりと、それぞれが別の役割を担っていました。

では、私達が使うプログラムのための役割を担ってくれるリテラルのようなものを作りたくないでしょうか？

例えば、**円柱が空間を占めているか、いないか**とか。

クラスの出番です！

担ってほしい約割を記述したクラスを作れば、リテラルのように自由に使えるようになります。

### オブジェクトとは

クラスを作成したら、クラスからオブジェクトを生成できます。意味がわからないと思います。

ですが、皆さんはすでにおこなっています。

まずは、下記を実行してみてください。

```rb
irb(main):003:0> "hoge".class
=> String
```

`.class`を使用することで、オブジェクトのクラスを確認できます。

つまり上記は`"hoge"`といる文字列が`String`クラスから生成されたオブジェクトということを示しています。

クラスのオブジェクトを生成するためには、`.new`を使用します。

```rb
b = String.new("hoge")
```

つまり、下記のように、`String.new`を使用するのと`""`で文字列を作成するのは全く同じことを行っているという意味です。

確認してみましょう。

```rb
a = "hoge"
b = String.new("hoge")
a == b
```

```rb
irb(main):019:0> a = "hoge"
=> "hoge"
irb(main):020:0> b = String.new("hoge")
=> "hoge"
irb(main):021:0> a == b
=> true
```

同じであることが確認できました！

つまり、リテラルとは、**あるクラスを楽に書く方法**だったのです。

`0`、`[]`、`{}`はすべてクラスがあります。

`.class`で確認してみてください。

### インスタンスとは

クラスからオブジェクトを生成することを**インスタンス化**(Instantiation)といいます。

Instantiationは具体化といった意味です。

文字列を扱うという抽象的なクラスから`"hoge"`とい文字列を使うように具体化したと考えると理解できるかと思います。

クラスからインスタンス化したオブジェクトを**インスタンス**(Instance)といいます。

あらためて説明すると、下記の場合は`a`がインスタンス、`String`がクラスです。意識しながら使っていきましょう。

```rb
a = String.new("hoge")
```

では、実際にクラスを定義して、そのオブジェクトを作成してみましょう。

### クラスの定義

> ⚠️  ここからは、何度も同名のクラスを定義しなおします。同名のクラスを再度定義する場合は、定義が上書きされるため、本講座と挙動が一致しない場合があります。そのようなことが起こった場合は、irbを起動し直してください。

ではあらためて、円柱が空間を占めているかいないかを判定できるクラスを作成してみましょう。

下記のように記述します。

```rb
class クラス名
end
```

クラス名は**キャメルケース**で記述してください。

キャメルケースは大文字英字始まりの英単語を結合して、文字列を組み立てる記法です。

`HogeHugaPiyo`のような形になります。

円柱を表すクラスを作成します。

```rb
class Cylinder
end
```

```rb
irb(main):029:1* class Cylinder
irb(main):030:0> end
=> nil
```

`Cylinder`クラスのインスタンスを作成してみましょう。

インスタンスを生成するためのメソッドはクラスで共通して`new`を使用します。

クラスを定義するだけで、Rubyが`new`メソッドを勝手に用意してくれると考えてください。

```rb
irb(main):031:0> cylinder = Cylinder.new()
=> #<Cylinder:0x0000559ec5637748>
```

`cylinder.class`で`cylinder`インスタンスのクラスを確認してみましょう。

```rb
irb(main):037:0> cylinder.class
=> Cylinder
```

### インスタンスメソッド

クラスは作成できましたが、これでは何もできません。

円柱の体積を計算できるようにしましょう。

クラス自体にメソッドを定義できます。

```rb
class クラス名
  def メソッド名(引数、引数...)
    処理
  end
end
```

では、`Cylinder`に体積を計算する`volume`メソッドを追加してみましょう。

```rb
class Cylinder
  def volume(radius, height)
    pi = 3.14
    radius ** 2 * pi * height
  end
end
```

```rb
irb(main):001:1* class Cylinder
irb(main):002:2*   def volume(radius, height)
irb(main):003:2*     pi = 3.14
irb(main):004:2*     radius ** 2 * pi * height
irb(main):005:1*   end
irb(main):006:0> end
=> :volume
```

では、円柱の体積を計算してみましょう。

```rb
irb(main):007:0> c = Cylinder.new
=> #<Cylinder:0x00005579885247a0>
irb(main):008:0> c.volume(5, 10)
=> 785.0
```

計算できていますね。

メソッドの引数がない場合は`()`を省略できるので`Cylinder.new`としています。

このようなインスタンスから呼び出せるメソッドを**インスタンスメソッド**といいます。

### コンストラクタ

しかし、このままだと、`c.volume(5, 10)`の結果を`785.0`はであることは確定しているのに、`volume(radius, height)`を実行するたびに体積の計算が行われてしまいます。

```rb
irb(main):010:0> c.volume(5, 10)
=> 785.0
irb(main):011:0> c.volume(5, 10) # <- 何度も同じ計算が、実行されてしまう
=> 785.0
irb(main):012:0> c.volume(5, 10) # <- 何度も同じ計算が、実行されてしまう
=> 785.0
```

なので、`Cylinder`クラスのインスタンス作成時に、体積の計算を行なうように変更していきます。

まず、クラスからインスタンスを生成するメソッドである`new`が引数を取れるように変更します。

現在のままだとエラーになります。

```rb
irb(main):007:0> Cylinder.new(5, 10)
(irb):7:in `initialize': wrong number of arguments (given 2, expected 0) (ArgumentError)
        from (irb):7:in `new'
        from (irb):7:in `<main>'
        from /usr/local/lib/ruby/gems/3.0.0/gems/irb-1.3.5/exe/irb:11:in `<top (required)>'
        from /usr/local/bin/irb:23:in `load'
        from /usr/local/bin/irb:23:in `<main>'
```

引数を取れるようするためには`initialize`というメソッドをクラスに定義する必要があります。

```rb
class クラス名
  def initialize(引数、引数...)
  end
  ...
end
```

```rb
class Cylinder
  def initialize(radius, height)
  end
end
```

先ほどまでの`Cylinder`クラスは`initialize`メソッドが定義されていませんでしが、引数のない`initialize`メソッドを勝手にRubyが用意してくれていたと考えてください。

では、`Cylinder`クラスをインスタンス化してみましょう。

```rb
class Cylinder
  def initialize(radius, height)
  end
end
```

```rb
irb(main):008:1* class Cylinder
irb(main):009:2*   def initialize(radius, height)
irb(main):010:1*   end
irb(main):011:0> end
=> :initialize
irb(main):012:0> c = Cylinder.new(5, 10)
=> #<Cylinder:0x0000562af2a6cbd8>
```

`new`の中で、`initialize`を実行してくれていると考えるとわかりやすいかもしれません。

`initialize`のようなインスタンス生成時の初期化に使うメソッドを**コンストラクタ**(Constructor)といいます。

### インスタンス変数

このままでは、インスタンス生成時に、引数を渡すだけで、インスタンス上に保存されていません。

なので、インスタンスの中に値を保存してみましょう。

下記のように保存します。

```rb
class Cylinder
  def initialize(radius, height)
    @radius = radius
    @height = height
  end
end
```

クラスのメソッド内で`@`からはじまる変数を代入によって作成するとインスタンス内で使用できます。

この変数名を**インスタンス変数**(Instance Variables)といいます。

上記では`@radius`に`radius`を代入しています。これは同じ名前である必要はありません。

`@hogehoge = radius`のような名前でも構いません。

あらためて構文を記載します。

```rb
class クラス名
  def initialize(引数1, 引数2...)
    @インスタンス変数名1 = 引数1
    @インスタンス変数名2 = 引数2
  end
end
```

実行してみましょう。

```rb
irb(main):022:1* class Cylinder
irb(main):023:2*   def initialize(radius, height)
irb(main):024:2*     @radius = radius
irb(main):025:2*     @height = height
irb(main):026:1*   end
irb(main):027:0> end
=> :initialize
irb(main):028:0>
irb(main):029:0> c = Cylinder.new(5, 10)
=> #<Cylinder:0x0000562af2e00dd0 @height=10, @radius=5>
```

`@height=10, @radius=5`と、インスタンス変数に代入ができていることがわかります。

### self

それでは、クラスに体積を計算するメソッドを追加し、`initialize`メソッドの中で呼び出すことで、インスタンス生成時に、体積を計算できるようにしましょう。

クラスに定義されたインスタンスメソッドを別のインスタンスメソッドから呼び出すためには`self`を使用します。

```rb
class クラス名
  def initialize
    self.何かをやるメソッド
  end

  def 何かをやるメソッド
  end
end
```

また、インスタンス変数はすでに宣言済みであれば、別のメソッドから使用することできます。

```rb
class クラス名
  def initialize
    @インスタンス変数 = 値
    self.何かをやるメソッド
  end

  def 何かをやるメソッド
    # initializeの中で、このメソッドを呼ぶ前に
    # @インスタンス変数 = 値
    # を行っているので
    # ここで@インスタンス変数を使用できる
  end
end
```

では、体積の計算を、インスタンス生成時行ってみましょう。

```rb
class Cylinder
  def initialize(radius, height)
    @radius = radius
    @height = height
    @volume = self.calculate_volume
  end

  def calculate_volume()
    pi = 3.14
    @radius ** 2 * pi * @height
  end
end
c = Cylinder.new(5, 10)
```

```rb
irb(main):091:1* class Cylinder
irb(main):092:2*   def initialize(radius, height)
irb(main):093:2*     @radius = radius
irb(main):094:2*     @height = height
irb(main):095:2*     @volume = self.calculate_volume
irb(main):096:1*   end
irb(main):097:1*
irb(main):098:2*   def calculate_volume()
irb(main):099:2*     pi = 3.14
irb(main):100:2*     @radius ** 2 * pi * @height
irb(main):101:1*   end
irb(main):102:0> end
=> :calculate_volume
irb(main):103:0> c = Cylinder.new(5, 10)
=> #<Cylinder:0x0000562af3127ae0 @height=10, @radius=5, @volume=785.0>
```

`@volume=785.0`と表示されています！できているようです！

### アクセスメソッド

体積が計算できたようですが、インスタンスの外から中のインスタンス変数にはアクセスできません。

```rb
c = Cylinder.new(5, 10)
c.volume #このようにアクセスしたい
```

そのためには、インスタンス変数と同名のメソッドを作成します。

```rb
class クラス名
  def initialize
    @インスタンス変数 = 値
  end

  def インスタンス変数
    @インスタンス変数
  end
end
```

`Cylinder`クラスの場合は、下記のようになります。

```rb
class Cylinder
  def initialize(radius, height)
    @radius = radius
    @height = height
    @volume = self.calculate_volume
  end

  def calculate_volume()
    pi = 3.14
    @radius ** 2 * pi * @height
  end

  def volume
    @volume
  end
end
```

では、再度定義をしなおし、インスタンスを生成して、`volume`メソッドを使ってみましょう。


```rb
irb(main):123:1* class Cylinder
irb(main):124:2*   def initialize(radius, height)
irb(main):125:2*     @radius = radius
irb(main):126:2*     @height = height
irb(main):127:2*     @volume = self.calculate_volume
irb(main):128:1*   end
irb(main):129:1*
irb(main):130:2*   def calculate_volume()
irb(main):131:2*     pi = 3.14
irb(main):132:2*     @radius ** 2 * pi * @height
irb(main):133:1*   end
irb(main):134:1*
irb(main):135:2*   def volume
irb(main):136:2*     @volume
irb(main):137:1*   end
irb(main):138:0> end
=> :volume
irb(main):139:0> c = Cylinder.new(5, 10)
irb(main):140:0> c.volume
=> 785.0
```

体積を出力できました!

`volume`メソッドは何度呼び出しても、計算をしなおすのではなく、同じ値を出力するようになりました！

でも、毎回インスタンス変数と同じメソッドを定義するのは面倒ですよね？これを簡単に定義できる方法があります。

`attr_reader`というものを使用します。

下記のように使用します。

```rb
class クラス名
  attr_reader :インスタンス変数
  def initialize
    @インスタンス変数 = 値
  end
end
```

書き直してみましょう。

```rb
class Cylinder
  attr_reader :volume

  def initialize(radius, height)
    @radius = radius
    @height = height
    @volume = self.calculate_volume
  end

  def calculate_volume()
    pi = 3.14
    @radius ** 2 * pi * @height
  end
end
```

```rb
irb(main):155:1* class Cylinder
irb(main):156:1*   attr_reader :volume
irb(main):157:1*
irb(main):158:2*   def initialize(radius, height)
irb(main):159:2*     @radius = radius
irb(main):160:2*     @height = height
irb(main):161:2*     @volume = self.calculate_volume
irb(main):162:1*   end
irb(main):163:1*
irb(main):164:2*   def calculate_volume()
irb(main):165:2*     pi = 3.14
irb(main):166:2*     @radius ** 2 * pi * @height
irb(main):167:1*   end
irb(main):168:0> end
=> :calculate_volume
irb(main):169:0> c = Cylinder.new(5, 10)
=> #<Cylinder:0x0000562af2ef4de0 @height=10, @radius=5, @volume=785.0>
irb(main):170:0> c.volume
=> 785.0
irb(main):171:0>
```

魔法のようですが、`attr_reader`を使用すると、裏でRubyが`volume`メソッドを定義してくれるようになると考えてください。

`attr_reader`は**アクセスメソッド**というものです。

| アクセスメソッド | 説明               |
| -                | -                  |
| attr_reader      | 読み取り           |
| attr_writer      | 書き込み           |
| attr_accessor    | 読み取りと書き取り |

`attr_writer`はインスタンス変数に値を保存するアクセスメソッドです。

例として、下記のようなクラスを用意します。


```rb
class User
  attr_writer :name
  attr_reader :name

  def initialize(name)
    @name = name
  end
end
```

```rb
irb(main):198:1* class User
irb(main):199:1*   attr_writer :name
irb(main):200:1*   attr_reader :name
irb(main):201:1*
irb(main):202:2*   def initialize(name)
irb(main):203:2*     @name = name
irb(main):204:1*   end
irb(main):205:0> end
=> :initialize
irb(main):206:0> u = User.new("taro")
=> #<User:0x0000562af2e2efc8 @name="taro">
irb(main):207:0> u.name
=> "taro"
irb(main):208:0> u.name = "hanako"  # <--------- これができるようになった。
=> "hanako"
irb(main):210:0> u.name
=> "hanako"
```

`attr_writer`を消して再度`User`クラスを定義し直してみてください。

代入できなくなります。

```rb
class User
  attr_reader :name

  def initialize(name)
    @name = name
  end
end
```

```rb
irb(main):008:0> u = User.new("taro")
=> #<User:0x0000557c9e1597b0 @name="taro">
irb(main):009:0> u.name = "hanako"
(irb):9:in `<main>': undefined method `name=' for #<User:0x0000557c9e1597b0 @name="taro"> (NoMethodError)
Did you mean?  name
        from /usr/local/lib/ruby/gems/3.0.0/gems/irb-1.3.5/exe/irb:11:in `<top (required)>'
        from /usr/local/bin/irb:23:in `load'
        from /usr/local/bin/irb:23:in `<main>'

```

``undefined method `name='``と表示されました。

つまり`attr_writer :name`を記述することは`name=`という名前のメソッドを定義すること同じです。

`name=`メソッドは`@name`に代入を行なう処理を定義します。

```rb
class User
  attr_reader :name

  def initialize(name)
    @name = name
  end

  def name=(name) # attr_writer :name と同じ
    @name = name
  end
end
```

```rb
irb(main):001:1* class User
irb(main):002:1*   attr_reader :name
irb(main):003:1*
irb(main):004:2*   def initialize(name)
irb(main):005:2*     @name = name
irb(main):006:1*   end
irb(main):007:1*
irb(main):008:2*   def name=(name)
irb(main):009:2*     @name = name
irb(main):010:1*   end
irb(main):011:0> end
=> :name=
irb(main):012:0> u = User.new("taro")
=> #<User:0x0000555dc3c6da88 @name="taro">
irb(main):013:0> u.name
=> "taro"
irb(main):014:0> u.name = "hanako"
=> "hanako"
irb(main):015:0> u.name
=> "hanako"
```

`attr_accessor`は、`attr_reader`と`attr_writer`を同じインスタンス変数に定義することと同効果があります。

```rb
class User
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end
```

```rb
irb(main):001:1* class User
irb(main):002:1*   attr_accessor :name
irb(main):003:1*
irb(main):004:2*   def initialize(name)
irb(main):005:2*     @name = name
irb(main):006:1*   end
irb(main):007:0> end
=> :initialize
irb(main):008:0> u = User.new("taro")
=> #<User:0x000055a10c2bd808 @name="taro">
irb(main):009:0> u.name                     # <- これもできる
=> "taro"
irb(main):010:0> u.name = "hanako"          # <- これもできる
=> "hanako"
irb(main):011:0> u.name
=> "hanako"
```

### ローカル変数

では、`Cylinder`クラスに戻りましょう。`Cylinder`クラスは現在このような定義です。

```rb
class Cylinder
  attr_reader :volume

  def initialize(radius, height)
    @radius = radius
    @height = height
    @volume = self.calculate_volume
  end

  def calculate_volume()
    pi = 3.14
    @radius ** 2 * pi * @height
  end
end
```

`calculate_volume`メソッドを見てください。

`pi`という変数はメソッドの内部で作成されています。

このような変数を**ローカル変数**といいます。ローカル変数はメソッドの内部でしかアクセスできません。

そして、`calculate_volume`が呼び出されるたびに何度も作成されます。


### 定数

これを変更して、毎回同じ変数を作成することを防ぎましょう。

定数はクラスの中に定義できます。

```rb
class クラス名
  定数 = 値
end
```

定数は大文字英数字と`_`で作成することが多いです。

`PIYO`、`HOGE_HUGA`のような形です。この記法をを**アッパースネークケース**といいます。


では、`Cylinder`クラスを変更します。

```rb
class Cylinder
  attr_reader :volume

  PI = 3.14

  def initialize(radius, height)
    @radius = radius
    @height = height
    @volume = self.calculate_volume
  end

  def calculate_volume()
    @radius ** 2 * PI * @height
  end
end
```

これで、`calculate_volume`を何度呼び出しても、`PI`は定数は使用するようになりました。

### クラスメソッド

この`PI`をインスタンスの外からも使えるようにしてみましょう。

```rb
class Cylinder
  attr_reader :volume

  PI = 3.14

  def initialize(radius, height)
    @radius = radius
    @height = height
    @volume = self.calculate_volume
  end

  def calculate_volume
    @radius ** 2 * PI * @height
  end

  def pi
    PI
  end
end
```

```rb
irb(main):001:1* class Cylinder
irb(main):002:1*   attr_reader :volume
irb(main):003:1*
irb(main):004:1*   PI = 3.14
irb(main):005:1*
irb(main):006:2*   def initialize(radius, height)
irb(main):007:2*     @radius = radius
irb(main):008:2*     @height = height
irb(main):009:2*     @volume = self.calculate_volume
irb(main):010:1*   end
irb(main):011:1*
irb(main):012:2*   def calculate_volume
irb(main):013:2*     @radius ** 2 * PI * @height
irb(main):014:1*   end
irb(main):015:1*
irb(main):016:2*   def pi
irb(main):017:2*     PI
irb(main):018:1*   end
irb(main):019:0> end
=> :pi
irb(main):020:0> c = Cylinder.new(5, 10)
=> #<Cylinder:0x000055d57fe6b4f0 @height=10, @radius=5, @volume=785.0>
irb(main):021:0> c.pi
=> 3.14
```

この`pi`というメソッドはインスタンス変数を一切使用していません。定数のみです。

なので、インスタンスを生成せずに、クラスから直接アクセスできるメソッドに変更してみましょう。

このようなメソッドを**クラスメソッド**といいます。

下記のように記述します。

```rb
class クラス名
  def self.クラスメソッド
    処理
  end
end
```

では`pi`メソッドをクラスメソッドに変更してみましょう。

```rb
class Cylinder
  attr_reader :volume

  PI = 3.14

  def initialize(radius, height)
    @radius = radius
    @height = height
    @volume = self.calculate_volume
  end

  def calculate_volume
    @radius ** 2 * PI * @height
  end

  def self.pi
    PI
  end
end
```

クラスメソッドはクラスから直接呼び出せます。

```rb
irb(main):001:1* class Cylinder
irb(main):002:1*   attr_reader :volume
irb(main):003:1*
irb(main):004:1*   PI = 3.14
irb(main):005:1*
irb(main):006:2*   def initialize(radius, height)
irb(main):007:2*     @radius = radius
irb(main):008:2*     @height = height
irb(main):009:2*     @volume = self.calculate_volume
irb(main):010:1*   end
irb(main):011:1*
irb(main):012:2*   def calculate_volume
irb(main):013:2*     @radius ** 2 * PI * @height
irb(main):014:1*   end
irb(main):015:1*
irb(main):016:2*   def self.pi
irb(main):017:2*     PI
irb(main):018:1*   end
irb(main):019:0> end
=> :pi
irb(main):020:0> Cylinder.pi
=> 3.14
```

インスタンス変数の使用の有無がそのメソッドをクラスメソッドかインスタンスメソッドどちらで定義するかの指標になります。

## クラスの継承

Rubyを深く理解するためには継承の理解がかかせません。

継承とはクラスの機能を引き継ぎ、新たなクラスを作成する仕組みです。

クラスは自作の役割を作成するためにあると記載しましたが、継承は、その役割を引き継ぎつつも、より具体的な役割を新たに付与できます。

例えば、Rubyに組み込まれている`Numeric`クラスというものがあります。

[class Numeric](https://docs.ruby-lang.org/ja/latest/class/Numeric.html)

このクラスは数値を扱うという役割を担っています。

しかし、`+`や`-`などの演算子は`Numeric`クラスを継承したクラスで定義されています。

(`=`を定義できることを説明したように、`+`や`-`も定義できます！)

では、`Numeric`クラスを継承したクラスは`Integer`クラスと`Folat`クラスです。

すでに使ったことがありますよね？数値リテラルのうち、整数と浮動小数点数です。

```rb
irb(main):001:0> a = 1
=> 1
irb(main):002:0> b = 1.0
=> 1.0
irb(main):003:0> a.class
=> Integer
irb(main):004:0> b.class
=> Float
```

継承もとのクラスを**スーパークラス**(Super Class)といいます。

継承したクラスを**サブクラス**(Sub Class)といいます。

`superclass`メソッドで調べることができます。

```rb
irb(main):005:0> a.class.superclass
=> Numeric
irb(main):006:0> b.class.superclass
=> Numeric
```

このように、`Numeric`クラスは数値を扱う役割を担っていて、それを継承した`Integer`クラスは整数を扱う役割を担っています。一方の`Float`クラスは浮動小数点数は扱う役割を担っています。

継承の段階が深くなればなるほど役割はより具体的になっていくと思ってください。

`superclass`をたどっていくとすべてのクラスは`BasicObject`にたどりつきます。

ぜひとも色々なクラスで調べてみてください。

```rb
irb(main):014:0> [].class.superclass.superclass
=> BasicObject
```

継承は下記のように行なうことができます。

```rb
class サブクラス名 < スーパークラス名
```

ここでは、自作したクラスの継承の方法を説明しません。継承はメリットが多くあるように見えますが、使うのが非常に難しいものであるためです。

プログラミングに精通したエンジニアでも、継承をうまく使いこなせず、可読性が低く、バグを生みやすいクラスを作成してしまうことがあります。

誤解を恐れず言うと、自作したクラスの継承を使わければならないユースケースは一つもありません。そのため、基本的にはシンプルなクラスを作成しましょう。

継承を使いたい唯一のケースは次のチャプターで説明する[ライブラリ](https://www.ruby-lang.org/ja/libraries/)内のクラスの継承です。

汎用的な機能をひとかたまりにして、簡単に使えるようにしてくれているものがライブラリです。データベースの接続ライブラリや、画像を描画するライブラリなどです。

継承を行なうのはこういったライブラリのような汎用的な機能のみに抑えることをプログラミング学習の初期は意識してみてください。

継承についての話は「継承より委譲」等で検索すると色々な考え方を学ぶことができます。

## おわりに

Rubyの機能は例のごとくここでは、すべてを網羅できません。しかし、今後も学習するうえでかぎとなるような要素はすべて詰め込んだつもりです。

プログラミング言語は、書きまくって読みまくることが何よりの上達です。ぜひ、色々なコードを書いてみましょう。

次のチャプターでは、実際にファイルにRubyをファイル上に記述し、小さなプログラムを作成していきます。

より深い情報についてはAppendixを参照してください。
