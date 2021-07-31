# Fizz Buzz

いよいよ本格的なプログラムを記述していきます。

Fizz Buzzです！


## セットアップ

`~/code/burning-tech`に移動してください。

```sh
$ cd ~/code/burning-tech
```

教材リポジトリをクローンします。

```sh
$ git clone https://github.com/burning-tech/fizzbuzz-training.git
$ cd fizzbuzz-training
```

最終的な成果物は`ans`ディレクトリにありますので動かなかったり、わからない箇所があれば参考にしてみてください。

本チャプターでもいつものように適宜、`git`管理をしてください。

```sh
$ rm -rf .git
$ git init
```

これに合わせてカレントディレクトリをVS Codeで開き、Remote Containerでコンテナを立ち上げてください。

前チャプターとコンテナの設定は変わりません。

RSpecがインストール済みになっています。

それでは始めましょう。

## Fizz Buzz

Fizz Buzzの問題を今一度確認しましょう。

> 1 から 100 までの数字を出力するプログラムを作成します。 ただし、3の倍数には数字の代わりに「Fizz」を出力し、5の倍数には「Buzz」を出力します。 3と5の両方の倍数である数の場合は「FizzBuzz」を印刷します。

このままプログラムにするには問題が大きすぎるので細かく分割していきましょう。

- 1から100までの数字を出力するプログラムを作成します。
- 3の倍数には数字の代わりに「Fizz」を出力し
- 5の倍数には「Buzz」を出力します。
- 3と5の両方の倍数である数の場合は「FizzBuzz」を印刷します。

出力をテストするのは問題をややこしくするだけなので、値を返すメソッドを作成することにしましょう。

- 1から100までの数字を返却するメソッド
- 3の倍数には数字の代わりに「Fizz」を返却
- 5の倍数には「Buzz」を返却
- 3と5の両方の倍数である数の場合は「FizzBuzz」を返却

かなり解きやすくなりました。

- 1から100までの数字を返却するメソッド

これは1から100までの数字をループすることになりそうです。

- 1のときは1を返却するメソッド
- 2のときは2を返却するメソッド
- 3のときはFizzを返却するメソッド
- ...

と細かく分割できます。

これは引数に、数字を引数として受け取り、値を返すメソッドとして実装するのが良さそうです。

- 引数として数字(1から100まで)を受け取り、値を返すメソッド
  - 引数が3の倍数のとき、"Fizz"を返却
  - 引数が5の倍数のとき、"Buzz"を返却
  - 引数が3,5両方の倍数のとき、"FizzBuzz"を返却
  - 引数が上記以外の場合はそのまま返却する

これでなんとなく、どんなふうに実装すればよいかわかったのではないでしょうか?

一つだけ修正しておきたい箇所があります。

引数や、返り値の型は基本的に常に同一であるべきです。

Rubyでは、異なる型を引数や返り値に受けることができますが、これを避けることでより安全なコードを記述できます。

下記のように修正しましょう。

- 引数として数字(1から100まで)を受け取り、値を返すメソッド
  - 引数が3の倍数のとき、"Fizz"を返却
  - 引数が5の倍数のとき、"Buzz"を返却
  - 引数が3,5両方の倍数のとき、"FizzBuzz"を返却
  - 引数が上記以外の場合は文字列に変換して返却する

上記を正しく実装するためにテストを一緒に書いていきます。

ここで開発スタイルのひとつ**TDD**を使ってテストを書いていきます。

## Test Driven Development

[Test Driven Development(テスト駆動開発 TDD)](https://ja.wikipedia.org/wiki/%E3%83%86%E3%82%B9%E3%83%88%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA)とはプログラミングにおける開発スタイルの一つのです。

下記のサイクルを繰り返す開発スタイルです。

1. レッド - パスしないテストを少しだけ作成する。
2. グリーン - テストをすぐにパスさせる。そのためには、どのようなコードでも良い。
3. リファクタリング - テストをパスさせるためだけに作成された重複をすべて取り除く。

**リファクタリング**とはテストは成功したままでコードの改善を行なうことです。

[テスト駆動開発](https://amzn.to/3vFYWFm)という原著では、このサイクルを繰り返すことで、「動作するきれいなコード」を目標にすると記載されています。

1. 動作しないコードを作成する。
2. 動作するコードに修正する。
3. 動作するきれいなコードに修正する。

動作するとはテストでパスをするということです。それほどテストが存在することは当たり前ということです。

ということです。

TDDを採用することで下記のような恩恵を受けることができます。

- 必ずテストが作成される
- 動くかわからないという時間を極限まで短くできる
- できるコードを用意にリファクタリングできる

それではレッド/グリーン/リファクタリングを繰り返して、Fizz Buzzプログラムを作成していきましょう。

## レッド

まずは`RSpec`の初期化をしましょう。

```sh
$ docker compose exec app bundle exec rspec --init
  create   .rspec
  create   spec/spec_helper.rb
```

`spec/spec_helper.rb`を前のチャプターと同様に修正してください。

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

今回はFizz Buzzコードを管理するために`lib`ディレクトリに`fizzbuzz.rb`を作成してください。

`spec`ディレクトリに`fizzbuzz_spec.rb`を作成してください。

```sh
$ mkdir lib
$ touch lib/fizzbuzz.rb
$ touch spec/fizzbuzz_spec.rb
```

ではテスト書いてきましょう。

TDDで開発するときに、一番ストレスがあるのは一回目のレッド/グリーン/リファクタリングのサイクルです。

1. テスト環境を作成する
2. コードを書くファイルを作成する
3. テストを書くファイルを作成する
4. メソッド名を考える
5. 必要であれば、クラス名を考える
6. テスト名を考える

やることがいっぱいあります。

なので最初のサイクルでは、実装する機能はもっとも楽なものを選ぶべきです。

- 引数として数字(1から100まで)を受け取り、値を返すメソッド
  - 引数が3の倍数のとき、"Fizz"を返却
  - 引数が5の倍数のとき、"Buzz"を返却
  - 引数が3,5両方の倍数のとき、"FizzBuzz"を返却
  - 引数が上記以外の場合は文字列に変換して返却する

おそらく、`引数が上記以外の場合は文字列に変換して返却する`がもっとも容易でしょう。

このテストから作成していきます。

まずは、`1. レッド - パスしないテストを少しだけ作成する`を行いましょう。

`spec/fizzbuzz_spec.rb`を下記のように修正してください。

```rb
require './lib/fizzbuzz'
require 'spec_helper'

RSpec.describe FizzBuzz do
    example "1を渡すと'1'を返却する" do
        fizzbuzz = FizzBuzz.new()
        expect(fizzbuzz.convert(1)).to eq "1"
    end
end
```

このテストは失敗します。

```sh
docker compose exec app bundle exec rspec

An error occurred while loading ./spec/fizzbuzz_spec.rb.
Failure/Error:
  RSpec.describe FizzBuzz do
    example "1を渡すと'1'を返却する" do
      fizzbuzz = FizzBuzz.new()
      expect(fizzbuzz.convert(1)).to eq "1"
    end
  end

NameError:
  uninitialized constant FizzBuzz
# ./spec/fizzbuzz_spec.rb:4:in `<top (required)>'
No examples found.

Top 0 slowest examples (0 seconds, 0.0% of total time):

Finished in 0.00004 seconds (files took 0.10511 seconds to load)
0 examples, 0 failures, 1 error occurred outside of examples
```

当たり前ですね。`uninitialized constant FizzBuzz`とメッセージが出ています。

つまり、`FizzBuzz`クラスをが存在しないためエラーになっています。

`1. レッド - パスしないテストを少しだけ作成する`ができました。

## グリーン

`2. グリーン - テストをすぐにパスさせる。そのためには、どのようなコードでも良い`を実行しましょう。

前回作成したテストでは下記が行われています。

1. `FizzBuzz`クラスのインスタンスを生成する
2. そのインスタンスメソッド`convert`に引数`1`を与えて、`"1"`が返却されることを期待する

つまり`FizzBuzz`クラスの作成と、`convert`インスタンスメソッドを作成しましょう。

`lib/fizzbuzz.rb`を下記のように修正してください。

```rb
class FizzBuzz
    def convert(number)
        "1"
    end
end
```

どんな値が引数に来ても返り値として`"1"`を返却しています。

コードしては問題がありますが、`2. グリーン - テストをすぐにパスさせる。そのためには、どのようなコードでも良い`とありますので、とにかくグリーンを目指します。

ここで、前回と同じエラーメッセージだったり、全く違うことが指摘される場合、なにか別の問題があるはずです。

最初のレッド/グリーン/リファクタリングのサイクルはストレスがもっとも大きいのでこまめに予想と実際の結果にずれがないかを確認しましょう。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 44912

FizzBuzz
  1を渡すと'1'を返却する

Top 1 slowest examples (0.00138 seconds, 19.5% of total time):
  FizzBuzz 1を渡すと'1'を返却する
    0.00138 seconds ./spec/fizzbuzz_spec.rb:5

Finished in 0.00705 seconds (files took 0.07949 seconds to load)
1 example, 0 failures

Randomized with seed 44912
```

```sh
1 example, 0 failures
```

`2. グリーン - テストをすぐにパスさせる。そのためには、どのようなコードでも良い`は完了です！

## リファクタリング

`3. リファクタリング - テストをパスさせるためだけに作成された重複をすべて取り除く。`を実行します。

重複はありませんが、リファクタリングを行なう余地は十分に残されています。

現状のままでは引数に`"1"`以外のときに対応していません。

`"1"`以外を期待するテストを作成しましょう。

`spec/fizzbuzz_spec.rb`を下記のように修正してください。

```diff
  require './lib/fizzbuzz'
  require 'spec_helper'

  RSpec.describe FizzBuzz do
      example "1を渡すと'1'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(1)).to eq "1"
      end
+
+     example "2を渡すと'2'を返却する" do
+         fizzbuzz = FizzBuzz.new()
+         expect(fizzbuzz.convert(2)).to eq "2"
+     end
  end
```

`"2"`の場合を追加しました。

テストを実行してみましょう。

失敗するはずです。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 59463

FizzBuzz
  1を渡すと'1'を返却する
  2を渡すと'2'を返却する (FAILED - 1)

Failures:

  1) FizzBuzz 2を渡すと'2'を返却する
     Failure/Error: expect(fizzbuzz.convert(2)).to eq "2"

       expected: "2"
            got: "1"

       (compared using ==)
     # ./spec/fizzbuzz_spec.rb:12:in `block (2 levels) in <top (required)>'

Top 2 slowest examples (0.03053 seconds, 83.1% of total time):
  FizzBuzz 2を渡すと'2'を返却する
    0.02934 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 1を渡すと'1'を返却する
    0.00119 seconds ./spec/fizzbuzz_spec.rb:5

Finished in 0.03676 seconds (files took 0.0945 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/fizzbuzz_spec.rb:10 # FizzBuzz 2を渡すと'2'を返却する

Randomized with seed 59463
```

予想通りですね。

ではリファクタリングを行いましょう。

`lib/fizzbuzz.rb`を下記のように修正してください。

```diff
  class FizzBuzz
      def convert(number)
-         "1"
+         number.to_s
      end
  end
```

`to_s`というメソッドは整数を文字列に変換するメソッドです。`Integer`クラスに実装されています。

ではテストを実行しましょう。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 14530

FizzBuzz
  2を渡すと'2'を返却する
  1を渡すと'1'を返却する

Top 2 slowest examples (0.0011 seconds, 12.7% of total time):
  FizzBuzz 2を渡すと'2'を返却する
    0.00101 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 1を渡すと'1'を返却する
    0.0001 seconds ./spec/fizzbuzz_spec.rb:5

Finished in 0.00865 seconds (files took 0.09695 seconds to load)
2 examples, 0 failures

Randomized with seed 14530
```

うまくいきました！

1. ひとつのテストだけに対応した実装をする
2. 2つ目のテストを作成する
3. 2つのテストに対応するように実装を抽象化する

というフローをTDDでは**三角測量**といいます。

三角測量において1のことを**仮実装**といいます。

厳密なTDDでは2つ以上のテスト(2つ以上の`examples`)があるときだけ実装を抽象化しても良いとなっています。

これでサイクルの一回目が完了しました！

では、2回目のサイクルに入ります。

## レッド2

- 引数として数字(1から100まで)を受け取り、値を返すメソッド
  - [ ] 引数が3の倍数のとき、"Fizz"を返却
  - [ ] 引数が5の倍数のとき、"Buzz"を返却
  - [ ] 引数が3,5両方の倍数のとき、"FizzBuzz"を返却
  - [x] 引数が上記以外の場合は文字列に変換して返却する

ひとつ実装できたので続いて、`引数が3の倍数のとき、"Fizz"を返却`に対応します。

`1. レッド - パスしないテストを少しだけ作成する`をやりましょう。

テストケースを追加しましょう。

`spec/fizzbuzz_spec.rb`を下記のように修正してください。

```diff
  require './lib/fizzbuzz'
  require 'spec_helper'

  RSpec.describe FizzBuzz do
      example "1を渡すと'1'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(1)).to eq "1"
      end

      example "2を渡すと'2'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(2)).to eq "2"
      end
+
+     example "3を渡すと'Fizz'を返却する" do
+         fizzbuzz = FizzBuzz.new()
+         expect(fizzbuzz.convert(3)).to eq "Fizz"
+     end
  end
```

テストを実行します。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 41813

FizzBuzz
  2を渡すと'2'を返却する
  1を渡すと'1'を返却する
  3を渡すと'Fizz'を返却する (FAILED - 1)

Failures:

  1) FizzBuzz 3を渡すと'Fizz'を返却する
     Failure/Error: expect(fizzbuzz.convert(3)).to eq "Fizz"

       expected: "Fizz"
            got: "3"

       (compared using ==)
     # ./spec/fizzbuzz_spec.rb:17:in `block (2 levels) in <top (required)>'

Top 3 slowest examples (0.02213 seconds, 77.9% of total time):
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.02075 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 2を渡すと'2'を返却する
    0.00121 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 1を渡すと'1'を返却する
    0.00016 seconds ./spec/fizzbuzz_spec.rb:5

Finished in 0.0284 seconds (files took 0.08702 seconds to load)
3 examples, 1 failure

Failed examples:

rspec ./spec/fizzbuzz_spec.rb:15 # FizzBuzz 3を渡すと'Fizz'を返却する
```

予想通り失敗しました。

## グリーン2

`2. グリーン - テストをすぐにパスさせる。そのためには、どのようなコードでも良い`を行います。

`lib/fizzbuzz.rb`を下記のように修正してください。

```diff
  class FizzBuzz
      def convert(number)
+         if number == 3 then
+             return "Fizz"
+         end
          number.to_s
      end
  end
```

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 30445

FizzBuzz
  1を渡すと'1'を返却する
  2を渡すと'2'を返却する
  3を渡すと'Fizz'を返却する

Top 3 slowest examples (0.00128 seconds, 17.1% of total time):
  FizzBuzz 1を渡すと'1'を返却する
    0.00105 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00013 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 2を渡すと'2'を返却する
    0.0001 seconds ./spec/fizzbuzz_spec.rb:10

Finished in 0.00747 seconds (files took 0.08048 seconds to load)
3 examples, 0 failures

Randomized with seed 30445
```

期待通りです！

## リファクタリング2

`3. リファクタリング - テストをパスさせるためだけに作成された重複をすべて取り除く。`を実行します。

三角測量をしましょう。

まずはテストを追加します。

```diff
  require './lib/fizzbuzz'
  require 'spec_helper'

  RSpec.describe FizzBuzz do
      example "1を渡すと'1'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(1)).to eq "1"
      end

      example "2を渡すと'2'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(2)).to eq "2"
      end

      example "3を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(3)).to eq "Fizz"
      end
+
+     example "6を渡すと'Fizz'を返却する" do
+         fizzbuzz = FizzBuzz.new()
+         expect(fizzbuzz.convert(6)).to eq "Fizz"
+     end
  end
```

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 12224

FizzBuzz
  1を渡すと'1'を返却する
  6を渡すと'Fizz'を返却する (FAILED - 1)
  3を渡すと'Fizz'を返却する
  2を渡すと'2'を返却する

Failures:

  1) FizzBuzz 6を渡すと'Fizz'を返却する
     Failure/Error: expect(fizzbuzz.convert(6)).to eq "Fizz"

       expected: "Fizz"
            got: "6"

       (compared using ==)
     # ./spec/fizzbuzz_spec.rb:22:in `block (2 levels) in <top (required)>'

Top 4 slowest examples (0.02255 seconds, 77.9% of total time):
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.02108 seconds ./spec/fizzbuzz_spec.rb:20
  FizzBuzz 1を渡すと'1'を返却する
    0.00128 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00011 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 2を渡すと'2'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:10

Finished in 0.02893 seconds (files took 0.08311 seconds to load)
4 examples, 1 failure

Failed examples:

rspec ./spec/fizzbuzz_spec.rb:20 # FizzBuzz 6を渡すと'Fizz'を返却する

Randomized with seed 12224
```

失敗しました。想定通りです!

では、`6を渡すと'Fizz'を返却する`、`3を渡すと'Fizz'を返却する`の両方に対応するために`convert`を抽象化しましょう。

`lib/fizzbuzz.rb`を下記のように修正してください。

```diff
  class FizzBuzz
      def convert(number)
-         if number == 3 then
+         if number % 3 == 0 then
              return "Fizz"
          end
          number.to_s
      end
  end
```

`number`を`3`で割ったときのあまりが`0`のときに`"Fizz"`を返却するようにしています。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 5949

FizzBuzz
  3を渡すと'Fizz'を返却する
  1を渡すと'1'を返却する
  6を渡すと'Fizz'を返却する
  2を渡すと'2'を返却する

Top 4 slowest examples (0.00149 seconds, 20.2% of total time):
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00099 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 1を渡すと'1'を返却する
    0.00019 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 2を渡すと'2'を返却する
    0.00018 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00013 seconds ./spec/fizzbuzz_spec.rb:20

Finished in 0.00739 seconds (files took 0.08262 seconds to load)
4 examples, 0 failures

Randomized with seed 5949
```

リファクタリング完了です。

## レッド3

- 引数として数字(1から100まで)を受け取り、値を返すメソッド
  - [x] 引数が3の倍数のとき、"Fizz"を返却
  - [ ] 引数が5の倍数のとき、"Buzz"を返却
  - [ ] 引数が3,5両方の倍数のとき、"FizzBuzz"を返却
  - [x] 引数が上記以外の場合は文字列に変換して返却する

`引数が5の倍数のとき、"Buzz"を返却`に対応しましょう。

`3`の倍数のときとほとんど変わりません。

`spec/fizzbuzz_spec.rb` を下記のように修正してください。

```diff
  require './lib/fizzbuzz'
  require 'spec_helper'

  RSpec.describe FizzBuzz do
      example "1を渡すと'1'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(1)).to eq "1"
      end

      example "2を渡すと'2'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(2)).to eq "2"
      end

      example "3を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(3)).to eq "Fizz"
      end

      example "6を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(6)).to eq "Fizz"
      end
+
+     example "5を渡すと'Buzz'を返却する" do
+         fizzbuzz = FizzBuzz.new()
+         expect(fizzbuzz.convert(5)).to eq "Buzz"
+     end
  end
```

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 50604

FizzBuzz
  6を渡すと'Fizz'を返却する
  1を渡すと'1'を返却する
  2を渡すと'2'を返却する
  3を渡すと'Fizz'を返却する
  5を渡すと'Buzz'を返却する (FAILED - 1)

Failures:

  1) FizzBuzz 5を渡すと'Buzz'を返却する
     Failure/Error: expect(fizzbuzz.convert(5)).to eq "Buzz"

       expected: "Buzz"
            got: "5"

       (compared using ==)
     # ./spec/fizzbuzz_spec.rb:27:in `block (2 levels) in <top (required)>'

Top 5 slowest examples (0.02114 seconds, 79.2% of total time):
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.0198 seconds ./spec/fizzbuzz_spec.rb:25
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00107 seconds ./spec/fizzbuzz_spec.rb:20
  FizzBuzz 2を渡すと'2'を返却する
    0.00009 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00009 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 1を渡すと'1'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:5

Finished in 0.02669 seconds (files took 0.08177 seconds to load)
5 examples, 1 failure

Failed examples:

rspec ./spec/fizzbuzz_spec.rb:25 # FizzBuzz 5を渡すと'Buzz'を返却する

Randomized with seed 50604
```

失敗を確認しました。

## グリーン3

`3`のときとほとんど同じなので少しだけサイクルを早めます。

`lib/fizzbuzz.rb`を下記のように修正してください。

```diff
  class FizzBuzz
      def convert(number)
          if number % 3 == 0 then
              return "Fizz"
          end
+
+         if number % 5 == 0 then
+             return "Buzz"
+         end
          number.to_s
      end
  end
```

`3`のときとほとんど同じなので、いきなり、`number`を`5`で割ったときのあまりが`0`のときに`"Buzz"`を返却するようにしています。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 36223

FizzBuzz
  2を渡すと'2'を返却する
  6を渡すと'Fizz'を返却する
  5を渡すと'Buzz'を返却する
  1を渡すと'1'を返却する
  3を渡すと'Fizz'を返却する

Top 5 slowest examples (0.00155 seconds, 19.8% of total time):
  FizzBuzz 2を渡すと'2'を返却する
    0.00105 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 1を渡すと'1'を返却する
    0.00021 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00013 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:20
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:25

Finished in 0.00785 seconds (files took 0.08532 seconds to load)
5 examples, 0 failures

Randomized with seed 36223
```

オールグリーンです。

## リファクタリング3

三角測量は2つ以上のテストケースをのときだけしか抽象化してはならなりません。

なので`10`のときのテストケースも追加します。

`spec/fizzbuzz_spec.rb`を下記のように修正してください。

```diff
  require './lib/fizzbuzz'
  require 'spec_helper'

  RSpec.describe FizzBuzz do
      example "1を渡すと'1'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(1)).to eq "1"
      end

      example "2を渡すと'2'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(2)).to eq "2"
      end

      example "3を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(3)).to eq "Fizz"
      end

      example "6を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(6)).to eq "Fizz"
      end

      example "5を渡すと'Buzz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(5)).to eq "Buzz"
      end
+
+     example "10を渡すと'Buzz'を返却する" do
+         fizzbuzz = FizzBuzz.new()
+         expect(fizzbuzz.convert(10)).to eq "Buzz"
+     end
  end
```

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 21865

FizzBuzz
  5を渡すと'Buzz'を返却する
  6を渡すと'Fizz'を返却する
  2を渡すと'2'を返却する
  1を渡すと'1'を返却する
  10を渡すと'Buzz'を返却する
  3を渡すと'Fizz'を返却する

Top 6 slowest examples (0.00205 seconds, 23.1% of total time):
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.00109 seconds ./spec/fizzbuzz_spec.rb:25
  FizzBuzz 10を渡すと'Buzz'を返却する
    0.00037 seconds ./spec/fizzbuzz_spec.rb:30
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00017 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 1を渡すと'1'を返却する
    0.00016 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00013 seconds ./spec/fizzbuzz_spec.rb:20
  FizzBuzz 2を渡すと'2'を返却する
    0.00013 seconds ./spec/fizzbuzz_spec.rb:10

Finished in 0.00888 seconds (files took 0.07984 seconds to load)
6 examples, 0 failures

Randomized with seed 21865
```

OKです。

テストを追加しただけで、リファクタリングでは何もコードに変更を加えていません。

別にそれでも良いのですが、改良の余地がないか見てみましょう。

```rb
class FizzBuzz
    def convert(number)
        if number % 3 == 0 then
            return "Fizz"
        end

        if number % 5 == 0 then
            return "Buzz"
        end
        number.to_s
    end
end
```

空白行が一箇所ないのが気になります。

```diff
  class FizzBuzz
      def convert(number)
          if number % 3 == 0 then
              return "Fizz"
          end

          if number % 5 == 0 then
              return "Buzz"
          end
+
          number.to_s
      end
  end
```

このレベルのリファクタリングでも、テストがあるため、完全なる自信をもって行なうことができます。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 44353

FizzBuzz
  10を渡すと'Buzz'を返却する
  2を渡すと'2'を返却する
  1を渡すと'1'を返却する
  6を渡すと'Fizz'を返却する
  5を渡すと'Buzz'を返却する
  3を渡すと'Fizz'を返却する

Top 6 slowest examples (0.00153 seconds, 20.8% of total time):
  FizzBuzz 10を渡すと'Buzz'を返却する
    0.00106 seconds ./spec/fizzbuzz_spec.rb:30
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00011 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 2を渡すと'2'を返却する
    0.0001 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.0001 seconds ./spec/fizzbuzz_spec.rb:25
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00009 seconds ./spec/fizzbuzz_spec.rb:20
  FizzBuzz 1を渡すと'1'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:5

Finished in 0.00734 seconds (files took 0.04953 seconds to load)
6 examples, 0 failures

Randomized with seed 44353
```

## レッド4

- 引数として数字(1から100まで)を受け取り、値を返すメソッド
  - [x] 引数が3の倍数のとき、"Fizz"を返却
  - [x] 引数が5の倍数のとき、"Buzz"を返却
  - [ ] 引数が3,5両方の倍数のとき、"FizzBuzz"を返却
  - [x] 引数が上記以外の場合は文字列に変換して返却する


最後の`引数が3,5両方の倍数のとき、"FizzBuzz"を返却`を対応しましょう。

これもほとんどかわりませんが、基本に忠実に、テストから追加していきます。

```diff
  require './lib/fizzbuzz'
  require 'spec_helper'

  RSpec.describe FizzBuzz do
      example "1を渡すと'1'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(1)).to eq "1"
      end

      example "2を渡すと'2'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(2)).to eq "2"
      end

      example "3を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(3)).to eq "Fizz"
      end

      example "6を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(6)).to eq "Fizz"
      end

      example "5を渡すと'Buzz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(5)).to eq "Buzz"
      end

      example "10を渡すと'Buzz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(10)).to eq "Buzz"
      end
+
+     example "15を渡すと'FizzBuzz'を返却する" do
+         fizzbuzz = FizzBuzz.new()
+         expect(fizzbuzz.convert(15)).to eq "FizzBuzz"
+     end
  end
```

テストを失敗させましょう。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 29984

FizzBuzz
  3を渡すと'Fizz'を返却する
  6を渡すと'Fizz'を返却する
  10を渡すと'Buzz'を返却する
  1を渡すと'1'を返却する
  5を渡すと'Buzz'を返却する
  15を渡すと'FizzBuzz'を返却する (FAILED - 1)
  2を渡すと'2'を返却する

Failures:

  1) FizzBuzz 15を渡すと'FizzBuzz'を返却する
     Failure/Error: expect(fizzbuzz.convert(15)).to eq "FizzBuzz"

       expected: "FizzBuzz"
            got: "Fizz"

       (compared using ==)
     # ./spec/fizzbuzz_spec.rb:37:in `block (2 levels) in <top (required)>'

Top 7 slowest examples (0.02159 seconds, 75.9% of total time):
  FizzBuzz 15を渡すと'FizzBuzz'を返却する
    0.02015 seconds ./spec/fizzbuzz_spec.rb:35
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00098 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 2を渡すと'2'を返却する
    0.00015 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:25
  FizzBuzz 1を渡すと'1'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 10を渡すと'Buzz'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:30
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:20

Finished in 0.02846 seconds (files took 0.08145 seconds to load)
7 examples, 1 failure

Failed examples:

rspec ./spec/fizzbuzz_spec.rb:35 # FizzBuzz 15を渡すと'FizzBuzz'を返却する

Randomized with seed 29984
```

## グリーン4

`引数が3,5両方の倍数のとき、"FizzBuzz"を返却`の条件の追加は少しだけ、追加場所に注意する必要があります。

`lib/fizzbuzz.rb`を下記のように修正してください。

```diff
  class FizzBuzz
      def convert(number)
          if number % 3 == 0 then
              return "Fizz"
          end

          if number % 5 == 0 then
              return "Buzz"
          end
+
+         if number % 15 == 0 then
+             return "FizzBuzz"
+         end

          number.to_s
      end
  end
```

テストを実行します。

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 42253

FizzBuzz
  10を渡すと'Buzz'を返却する
  1を渡すと'1'を返却する
  2を渡すと'2'を返却する
  6を渡すと'Fizz'を返却する
  5を渡すと'Buzz'を返却する
  15を渡すと'FizzBuzz'を返却する (FAILED - 1)
  3を渡すと'Fizz'を返却する

Failures:

  1) FizzBuzz 15を渡すと'FizzBuzz'を返却する
     Failure/Error: expect(fizzbuzz.convert(15)).to eq "FizzBuzz"

       expected: "FizzBuzz"
            got: "Fizz"

       (compared using ==)
     # ./spec/fizzbuzz_spec.rb:37:in `block (2 levels) in <top (required)>'

Top 7 slowest examples (0.02069 seconds, 73.8% of total time):
  FizzBuzz 15を渡すと'FizzBuzz'を返却する
    0.01904 seconds ./spec/fizzbuzz_spec.rb:35
  FizzBuzz 10を渡すと'Buzz'を返却する
    0.00112 seconds ./spec/fizzbuzz_spec.rb:30
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.00015 seconds ./spec/fizzbuzz_spec.rb:25
  FizzBuzz 1を渡すと'1'を返却する
    0.00014 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00009 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 2を渡すと'2'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00007 seconds ./spec/fizzbuzz_spec.rb:20

Finished in 0.02803 seconds (files took 0.08496 seconds to load)
7 examples, 1 failure

Failed examples:

rspec ./spec/fizzbuzz_spec.rb:35 # FizzBuzz 15を渡すと'FizzBuzz'を返却する

Randomized with seed 42253
```

失敗してしまいました！

```sh
       expected: "FizzBuzz"
            got: "Fizz"
```

なぜなら、15は15の倍数ではもちろんあるのですが、5の倍数、3の倍数でもあります。

そのため、`3`の倍数のチェックに最初に引っかかってしまい、`Fizz`が返却されてしまいました。

これを防ぐために`15`のチェックを一番最初に行なう必要があります。

`lib/fizzbuzz.rb`を下記のように修正してください。

```diff
class FizzBuzz
    def convert(number)
+       if number % 15 == 0 then
+           return "FizzBuzz"
+       end
+
        if number % 3 == 0 then
            return "Fizz"
        end

        if number % 5 == 0 then
            return "Buzz"
        end

-       if number % 15 == 0 then
-           return "FizzBuzz"
-       end
-
        number.to_s
    end
end
```

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 20098

FizzBuzz
  3を渡すと'Fizz'を返却する
  6を渡すと'Fizz'を返却する
  5を渡すと'Buzz'を返却する
  10を渡すと'Buzz'を返却する
  15を渡すと'FizzBuzz'を返却する
  1を渡すと'1'を返却する
  2を渡すと'2'を返却する

Top 7 slowest examples (0.00146 seconds, 18.0% of total time):
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00104 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 15を渡すと'FizzBuzz'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:35
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:20
  FizzBuzz 10を渡すと'Buzz'を返却する
    0.00007 seconds ./spec/fizzbuzz_spec.rb:30
  FizzBuzz 1を渡すと'1'を返却する
    0.00007 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 2を渡すと'2'を返却する
    0.00006 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.00005 seconds ./spec/fizzbuzz_spec.rb:25

Finished in 0.00812 seconds (files took 0.08238 seconds to load)
7 examples, 0 failures

Randomized with seed 20098
```

テストにパスしました！

このようにテストを直ぐに実行するのでフィードバックを即座に受け取ることできます。

プログラムがテストに通らない不安な時間は非常に短くて済みます。

## リファクタリング4

三角測量のルールに従い、`FizzBuzz`を返却する2つ目のテストコードを追加しましょう。

`spec/fizzbuz_spec.rb`を下記のように修正してください。

```diff
  require './lib/fizzbuzz'
  require 'spec_helper'

  RSpec.describe FizzBuzz do
      example "1を渡すと'1'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(1)).to eq "1"
      end

      example "2を渡すと'2'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(2)).to eq "2"
      end

      example "3を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(3)).to eq "Fizz"
      end

      example "6を渡すと'Fizz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(6)).to eq "Fizz"
      end

      example "5を渡すと'Buzz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(5)).to eq "Buzz"
      end

      example "10を渡すと'Buzz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(10)).to eq "Buzz"
      end

      example "15を渡すと'FizzBuzz'を返却する" do
          fizzbuzz = FizzBuzz.new()
          expect(fizzbuzz.convert(15)).to eq "FizzBuzz"
      end
+
+     example "30を渡すと'FizzBuzz'を返却する" do
+         fizzbuzz = FizzBuzz.new()
+         expect(fizzbuzz.convert(30)).to eq "FizzBuzz"
+     end
  end
```

```sh
$ docker compose exec app bundle exec rspec

Randomized with seed 24524

FizzBuzz
  3を渡すと'Fizz'を返却する
  6を渡すと'Fizz'を返却する
  1を渡すと'1'を返却する
  15を渡すと'FizzBuzz'を返却する
  2を渡すと'2'を返却する
  30を渡すと'FizzBuzz'を返却する
  5を渡すと'Buzz'を返却する
  10を渡すと'Buzz'を返却する

Top 8 slowest examples (0.00152 seconds, 18.9% of total time):
  FizzBuzz 3を渡すと'Fizz'を返却する
    0.00098 seconds ./spec/fizzbuzz_spec.rb:15
  FizzBuzz 10を渡すと'Buzz'を返却する
    0.00012 seconds ./spec/fizzbuzz_spec.rb:30
  FizzBuzz 6を渡すと'Fizz'を返却する
    0.00009 seconds ./spec/fizzbuzz_spec.rb:20
  FizzBuzz 2を渡すと'2'を返却する
    0.00008 seconds ./spec/fizzbuzz_spec.rb:10
  FizzBuzz 1を渡すと'1'を返却する
    0.00007 seconds ./spec/fizzbuzz_spec.rb:5
  FizzBuzz 15を渡すと'FizzBuzz'を返却する
    0.00007 seconds ./spec/fizzbuzz_spec.rb:35
  FizzBuzz 30を渡すと'FizzBuzz'を返却する
    0.00007 seconds ./spec/fizzbuzz_spec.rb:40
  FizzBuzz 5を渡すと'Buzz'を返却する
    0.00006 seconds ./spec/fizzbuzz_spec.rb:25

Finished in 0.00806 seconds (files took 0.08597 seconds to load)
8 examples, 0 failures

Randomized with seed 24524
```

オールグリーンです！

- 引数として数字(1から100まで)を受け取り、値を返すメソッド
  - [x] 引数が3の倍数のとき、"Fizz"を返却
  - [x] 引数が5の倍数のとき、"Buzz"を返却
  - [x] 引数が3,5両方の倍数のとき、"FizzBuzz"を返却
  - [x] 引数が上記以外の場合は文字列に変換して返却する

上記が完成しました！

## 1から100まで出力してみる。

では実際に`FizzBuzz`クラスを使って、1から100まで正しく変換されるか出力を見てみましょう。

`fizzbuzz.rb`を作成してください。

下記のように編集してください。

```rb
require './lib/fizzbuzz'

fizzbuzz = FizzBuzz.new()

(1..100).each do |number|
    result = fizzbuzz.convert(number)
    puts "#{number} => #{result}"
end
```

戦略としては、1から100までをループし、それぞれ、`convert`メソッドを呼び出して結果を表示しています。

`(1..100)`の`..`は`Range`クラスのインスタンスの生成リテラルです。

`Range`という名前の通り、範囲を示し、ループ処理に使うことができます。

`(1..100`は1から100までの範囲という意味です。

実行してみましょう。

```sh
$ docker compose exec app ruby fizzbuzz.rb
1 => 1
2 => 2
3 => Fizz
4 => 4
5 => Buzz
6 => Fizz
7 => 7
8 => 8
9 => Fizz
10 => Buzz
11 => 11
12 => Fizz
13 => 13
14 => 14
15 => FizzBuzz
16 => 16
17 => 17
..
89 => 89
90 => FizzBuzz
91 => 91
92 => 92
93 => Fizz
94 => 94
95 => Buzz
96 => Fizz
97 => 97
98 => 98
99 => Fizz
100 => Buzz
```

`FizzBuzz`問題を解くことができました！

## おわりに

TDDのパワーを実感できたでしょうか?

気を抜くとテストコードを先に書くのを忘れてしまいます。

是非、習慣化するまでレッド/グリーン/リファクタリングを意識してTDDを実践してみてください。

その他、テストについてのノウハウやパターン等はAppendixを参照してください。
