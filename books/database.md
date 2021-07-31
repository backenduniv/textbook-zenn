# Relational Database

## リレーショナルデータベースとは

データベースとはデータを蓄積し、検索や変更を高速にできるソフトウェアです。

リレーショナルデータベースは現在もっとも広く普及しているデータベースです。

簡単に説明すると、大量のデータを格納できて、超高速なExcelです。

瞬時に検索をできます。

Excelのようにシートの行(**Row**)と列(**Column**)があります。

rowとcolumnを指定することで値を取得できます。

シートのことをRDBでは**Table**(テーブル)といいます。

シートにおける一行一行をRDBでは、レコードといいます。

レコードの中のカラムで特定できるひとつの値をフィールドといいます。

```
                     table A

                ↓   ↓   ↓   ↓   ↓
                      column
                ↓   ↓   ↓   ↓   ↓
          +---+---+---+---+---+---+
          |   | A | B | C | D | E |
          +---+---+---+---+---+---+
->     -> | 1 |   |   |   |   |   | ->     ->
          +---+---+---+---+---+---+
->     -> | 2 |   |   |   |   |   | ->     ->
          +---+---+---+---+---+---+
-> row -> | 3 |   |   |   |   |   | -> row ->
          +---+---+---+---+---+---+
->     -> | 4 |   |   | x |   |   | ->     ->
          +---+---+---+---+---+---+
->     -> | 5 |   |   |   |   |   | ->     ->
          +---+---+---+---+---+---+
                ↓   ↓   ↓   ↓   ↓
                      column
                ↓   ↓   ↓   ↓   ↓


                  x = field
```

複数のシートが格納されたExcelファイルをRDBでは**Database**(データベース)といいます。

```
      Database 1

 table A     table B
+---+---+   +---+---+
|   | A |   | C | D |
+---+---+   +---+---+
| 1 |   |   |   |   |
+---+---+   +---+---+

 table C     table D
+---+---+   +---+---+
| 3 |   |   |   |   |
+---+---+   +---+---+
| 4 |   |   | x |   |
+---+---+   +---+---+
```

RDBには複数の実装がありそれぞれの実装を**RDBMS**(Relational Database Management System)といいます。

本講座では**RDBMS**の一つである**MySQL**を使用していきます。

これで、最低限の説明は十分です。手を動かして、理解していきましょう。

## セットアップ

今回からすべてのセットアップをDockerを使用していきます。

`~/code/burning-tech`に移動してください。

```sh
$ cd ~/code/burning-tech
```

教材リポジトリをダウンロードします。

`git clone`というコマンドで行います。

`git clone`はremoteリポジトリを現在のディレクトリ配下にダウンロードします。

```sh
$ git clone https://github.com/burning-tech/rdbms-training.git
$ cd rdbms-training
```

みなさん自身のリポジトリで管理してほしいので`.git`を削除して、`git init`しましょう。

```sh
$ rm -rf .git
$ git init
```

適宜remoteリポジトリをみなさんのGitHubアカウントに作成して、`git push`等してください。

続いて、`mysql`を立ち上げるために、下記を実行してください。

```sh
# Dockerを起動していない場合は起動も必要です。
$ docker compose up -d
```

`-d`は実行をバックグラウンドで行います。つまり何も表示されませんが、裏でやってくれているということです。

`docker compose ps`でちゃんと動いているのか確認できます。

`State`が`running`だったらOKです。

```sh
$ docker compose ps
NAME                  SERVICE             STATUS              PORTS
rdbms-training_db_1   db                  running             0.0.0.0:3306->3306/tcp, 33060/tcp
```

MySQLを直接操作してRDBとはどのようなものなのか体感していきましょう。

`docker compose exec`はすでに動いているコンテナに対して何かを実行するコマンドです。

`docker compose exec db mysql`は`db`というサービス(`docker-compose.yml`で定義されている)に`mysql`コマンドを実行しているという意味になります。

```sh
$ docker compose exec db mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.25-log MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

MySQLを操作できるようになりました。

本チャプターで扱うSQLはすべて下記に記述されています。

<https://github.com/burning-tech/rdbms-training/tree/main/src>

### MySQLサーバーとMySQLクライアント

あなたが今見ている画面は**MySQLクライアント**というものです。

実際にデータが格納されているデータベースを**MySQLサーバー**といいます。

MySQLクライアントからMySQLサーバーに対してデータの操作や定義をします。

データの操作や定義を行なうための言語を**SQL**といいます。

実際にSQLを実行してみましょう。

### exampleデータのロード

下記コマンドを実行してください。

```sql
mysql> source src/example.sql
```

`source`コマンドはSQLファイルを実行するコマンドです。

わたしが事前に用意したSQLを実行し、講座用のデータをデータベースに入れました。

複数の会社に所属する従業員の情報を表すデータです。

このデータには3つのテーブルがあります。

- 会社テーブル
- 部署テーブル
- 従業員テーブル

3つのシートがExcelファイルの中にあるのを想像してください。

実際にそれぞれのテーブルにどんな情報が入っているのかを見てみましょう。

まず、`companies`という会社をテーブルのデータです。

下記のコマンドを実行して確認してください。

(SQLの末尾には`;`をつけるのを忘れないようにしてください。)

```sql
mysql> SELECT * FROM companies;
```

下記のような結果が出力されます。

```sql
mysql> SELECT * FROM companies;
+----+---------------------------+
| id | name                      |
+----+---------------------------+
|  1 | A銀行                     |
|  2 | B商社                     |
|  3 | Cホールディングス         |
|  4 | D出版                     |
+----+---------------------------+
```

続いて、`departments`という部署のテーブルのデータです。

下記のコマンドを実行して確認してください。

```sql
mysql> SELECT * FROM departments;
+----+------------+-----------------------------+
| id | company_id | name                        |
+----+------------+-----------------------------+
|  1 |          1 | 融資課                      |
|  2 |          1 | 外為課                      |
|  3 |          2 | 資材部                      |
|  4 |          2 | 物流部                      |
|  5 |          3 | 企画部                      |
|  6 |          3 | コンプライアンス部          |
|  7 |          4 | 編集部                      |
|  8 |          4 | 販売促進部                  |
+----+------------+-----------------------------+
```

```sql
mysql> SELECT * FROM employees;
+----+---------------+------------+-----------+
| id | department_id | first_name | last_name |
+----+---------------+------------+-----------+
|  1 |             1 | 太郎       | 田中      |
|  2 |             1 | 花子       | 山田      |
|  3 |             2 | 健太       | 高橋      |
|  4 |             2 | 真弓       | 鈴木      |
|  5 |             3 | 若菜       | 伊藤      |
|  6 |             3 | 三郎       | 斎藤      |
|  7 |             4 | 翔吾       | 木村      |
|  8 |             4 | 純         | 櫻井      |
|  9 |             5 | 一成       | 大野      |
| 10 |             5 | 将暉       | 二宮      |
| 11 |             6 | 聡         | 松本      |
| 12 |             6 | 翔         | 相葉      |
| 13 |             7 | 薫         | 橋本      |
| 14 |             7 | 麻里佳     | 北川      |
| 15 |             8 | 遼太       | 沢田      |
| 16 |             8 | 五郎       | 山本      |
+----+---------------+------------+-----------+
```

Excelのように行と列が並んでいるのを確認できたと思います。

`deparmtnets`テーブルには`company_id`というどの会社の部署かを示すidが振られています。

この`company_id`が`companies`テーブルのいずれかのレコード(行)の`id`フィールドと一致していれば、その会社の部署という意味になります。

`employees`テーブルの`department_id`も`departments`テーブルの`id`と対応しています。

### SQLを体感する

今は内容を完全に理解する必要はありません。何をやっているのか、どんなことができるのかを理解してください。

では、`山本五郎`さんがどの会社のどの部署にいるか検索してみましょう。

なれないうちはコピーペーストで構いません。

(＊ `->`は入力しないでください。改行すると勝手に表示されます。)

```sql
mysql> SELECT c.name '会社',
    ->        d.name '部署',
    ->        e.last_name '姓',
    ->        e.first_name '名'
    -> FROM employees e
    -> JOIN departments d ON d.id = e.department_id
    -> JOIN companies c ON c.id = d.company_id
    -> WHERE e.last_name = '山本'
    ->   AND e.first_name = '五郎';
+---------+-----------------+--------+--------+
| 会社    | 部署            | 姓     | 名     |
+---------+-----------------+--------+--------+
| D出版   | 販売促進部      | 山本   | 五郎   |
+---------+-----------------+--------+--------+
1 row in set (0.01 sec)
```

では、`A銀行`には何人従業員が所属しているか確認してみましょう。

```sql
mysql> SELECT COUNT(e.id) '従業員数'
    -> FROM employees e
    -> JOIN departments d ON d.id = e.department_id
    -> JOIN companies c ON c.id = d.company_id
    -> WHERE c.name = 'A銀行'
    -> GROUP BY c.id;
+--------------+
| 従業員数     |
+--------------+
|            4 |
+--------------+
1 row in set (0.00 sec)
```
(実際に、計算してみて、正しいか確認してみてください。)

最後に1から9までの漢数字のいずれかが名前に含まれている人を検索してみましょう。

```sql
mysql> SELECT e.last_name '姓',
    ->        e.first_name '名'
    -> FROM employees e
    -> JOIN departments d ON d.id = e.department_id
    -> JOIN companies c ON c.id = d.company_id
    -> WHERE e.last_name REGEXP '(一|二|三|四|五|六|七|八|九)'
    ->   OR e.first_name REGEXP '(一|二|三|四|五|六|七|八|九)';
+--------+--------+
| 姓     | 名     |
+--------+--------+
| 斎藤   | 三郎   |
| 大野   | 一成   |
| 二宮   | 将暉   |
| 山本   | 五郎   |
+--------+--------+
4 rows in set (0.00 sec)
```

このように、かなり柔軟に必要なデータを抽出して、意味のあるデータに変換できるのがSQLのパワーです。

1. データベースの作成
2. テーブルの作成
3. レコードの挿入
4. レコードの取得
5. レコードの更新
6. レコードの削除
7. リレーション

と順に学んでいきます。

## データベースの作成

[SQL](https://github.com/burning-tech/rdbms-training/blob/main/src/create_database.sql)

さきほどのexampleデータは`example`というデータベースを作成していました。

現在どのようなデータベースがあるか確認してみましょう。

```sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| example            |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.02 sec)
```

`information_schema`、`mysql`、`performance_schema`、`sys`はシステムが管理するデータベースです。

なので基本的に使わないようにしてください。

`example`データベースは`source src/example.sql`コマンドで作成したものです。

では、ここで今後操作していくためのデータベース`app`を作成しましょう。

`CREATE DATABASE`で作成できます。

```sql
mysql> CREATE DATABASE app;
Query OK, 1 row affected (0.01 sec)
```

データベースを使用するためには使用するデータベースを選択する必要があります。

現在選択中のデータベースを表示してみましょう。

```sql
mysql> SELECT database();
+------------+
| database() |
+------------+
| example    |
+------------+
1 row in set (0.00 sec)
```

選択中のデータベースが`example`データベースなので、`app`データベースを選択しましょう。

`USE`を使用します。

```sql
mysql> USE app;
Database changed
```

もう一度、現在選択しているデータベースを表示して`app`であることを確認しましょう。

```sql
mysql> SELECT database();
+------------+
| database() |
+------------+
| app        |
+------------+
1 row in set (0.00 sec)
```

## テーブルの作成

[SQL](https://github.com/burning-tech/rdbms-training/blob/main/src/create_table.sql)

テーブルを作成する前にデータ型というものについて説明します。

### データ型

`example`データベースの`companies`テーブルには`id`と`name`というカラム(列)がありました。

```sql
+----+---------------------------+
| id | name                      |
+----+---------------------------+
|  1 | A銀行                     |
|  2 | B商社                     |
|  3 | Cホールディングス         |
|  4 | D出版                     |
+----+---------------------------+
```

`id`フィールドには数字が入っていて、`name`フィールドには文字列が入っています。

文字列とは単語や文章のような文字が連続したものを指します。

このようにカラムごとに入力データの種類を分類することことができます。

この、数字や文字列などの分類を**データ型**といいます。

データ型はコンピュータが高速に処理をしたり、効率良くデータを処理するのをサポートしたり、安全性を高めたりと、多くの恩恵をもたらします。

MySQLにはデータ型が非常に多くあります。ここでは基本的なデータ型を4つだけ紹介します。

1. INT型     => 整数型

2. VARCHAR   => 可変長(長さが一定ではない)文字列

3. TEXT      => 巨大なテキストデータ型

4. DATETIME => 日付と時刻を扱う型

このようなデータ型についての詳細な学習は、ひとつ何かのプログラミング言語の習得が完了してからで良いと思います。

では、この中のうち`INT`、`VARCHAR`、`DATETIME`を使用してテーブルを作成していきましょう。

### 作成

`CREATE TABLE`で作成します。

`carts`というテーブルを作成しましょう。

このテーブルは通販サイトのショッピングカートを表したテーブルです。

```sql
mysql> CREATE TABLE carts (
    ->   id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    ->   user_name VARCHAR(255) NOT NULL,
    ->   start_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
    -> );
Query OK, 0 rows affected (0.04 sec)
```

作成が完了しました。

やっていることを一つ一つテーブルの定義を見ながら説明していきます。

テーブルの定義は`DESCRIBE`で見ることができます。

```sql
mysql> DESCRIBE carts;
+-----------+--------------+------+-----+-------------------+----------------+
| Field     | Type         | Null | Key | Default           | Extra          |
+-----------+--------------+------+-----+-------------------+----------------+
| id        | int(11)      | NO   | PRI | NULL              | auto_increment |
| user_name | varchar(255) | NO   |     | NULL              |                |
| start_at  | datetime     | NO   |     | CURRENT_TIMESTAMP |                |
+-----------+--------------+------+-----+-------------------+----------------+
3 rows in set (0.00 sec)
```

このテーブルには3つのカラムがあります。
そして、下記のような使い方を想定しているとします。

- id          ショッピングカートを識別するためのID
- user_name   ショッピングカートを使用しているユーザー名
- start_at    ショッピングが開始された時刻を示す

まず、idカラムからみていきましょう。

```sql
id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
```

#### INT

idカラムのデータ型は`INT`です。それ以降はデータ型以外の設定項目です。

#### AUTO_INCREMENT

とは日本語にすると自動増加のような意味です。

つまり`id`フィールドはレコードの作成によって自動で採番され、すでに存在する値よりひとつだけ大きい整数が割り当てられます。

レコードがない場合は1が入り、2、3と順に続きます。

#### NOT NULL

値が入っていないことを許容しない設定です。

#### PRIMARY KEY

レコードを識別するためのカラムであること示す設定です。

この設定がされているフィールドは重複することが許されない、つまり1が複数入るようなこと許されません。

テーブルには必ず一つのみ設定します。

このあとみていく`user_name`は他の人とかぶる可能性があるのでこのようなカラムを用意する必要があります。

続いて、user_nameカラムです。

```sql
user_name VARCHAR(255) NOT NULL,
```

#### VARCHAR(255)

user_nameカラムのデータ型は`VARCHAR(255)`です。

`255`は格納する最大文字数を表しています。

`VARCHAR`可変長文字列の可変長とは文字列のサイズが計算されてそのサイズで保存領域が確保されます。

そのため、データベースを節約できます。反面、作成時や取り出し時にサイズの計算が必要なので固定長(`CHAR`)より処理に時間がかかります。

最後に、start_atカラムです。

```sql
start_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
```

start_atカラムのデータ型は`DATETIME`です。

`DATETIME`は日付と時刻を格納するためのデータ型です。

フォーマットは`yyyy-MM-dd hh:mm:ss`です。

#### DEFAULT

デフォルト値を持つことを示します。その次にデフォルト値が続きます。

#### CURRENT_TIMESTAMP

これがデフォルト値となり、現在時刻を示します。

つまり、値を指定されなかったら現在時刻が入ります。

```sql
CREATE TABLE carts (
  id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
  user_name VARCHAR(255) NOT NULL,
  start_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

これでこのSQLが理解できるようになりました。

続いてこのテーブルにデータを入れていきます。

## レコードの挿入

[SQL](https://github.com/burning-tech/rdbms-training/blob/main/src/insert_record.sql)

レコードの挿入には`INSERT INTO`を使用します。

```sql
mysql> INSERT INTO carts (user_name) VALUES ('taro');
Query OK, 1 row affected (0.00 sec)
```

```sql
INSERT INTO テーブル名 (カラム名) VALUES (値)
```

今回使用したテーブルは`id`カラムが`AUTO_INCREMENT`、`start_at`カラムが`DEFAULT CURRENT_TIMESTAMP`だったのでカラムを一つだけ指定して`INSERT`しました。

複数の値を指定したい場合は下記のようにカンマ区切りで記述します。

```sql
mysql> INSERT INTO carts (user_name, start_at) VALUES ('hanako', CURRENT_TIMESTAMP);
Query OK, 1 row affected (0.00 sec)
```

複数のレコードを同時に`INSERT`できます。`INSERT`するレコードをカンマ区切りで記述します。

```sql
mysql> INSERT INTO carts (user_name) VALUES
    ->   ('jiro'),
    ->   ('saburo'),
    ->   ('shiro'),
    ->   ('goro');
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0
```

## レコードの取得

[SQL](https://github.com/burning-tech/rdbms-training/blob/main/src/select_record.sql)

それでは`INSERT`したレコードを取得してみましょう。

`SELECT`を使用します。

```sql
mysql> SELECT id
    -> FROM carts;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  4 |
|  5 |
|  6 |
+----+
6 rows in set (0.01 sec)
```

```sql
SELECT カラム名 FROM テーブル名;
```

`SELECT`の後に取得したいカラム名を並べて、`FROM`の後に対象のテーブルを記述します。

すべてのカラムを取得してみましょう。

```sql
mysql> SELECT id,
    ->        user_name,
    ->        start_at
    -> FROM carts;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | taro      | 2021-05-09 02:14:07 |
|  2 | hanako    | 2021-05-09 02:14:07 |
|  3 | jiro      | 2021-05-09 02:16:12 |
|  4 | saburo    | 2021-05-09 02:16:12 |
|  5 | shiro     | 2021-05-09 02:16:12 |
|  6 | goro      | 2021-05-09 02:16:12 |
+----+-----------+---------------------+
6 rows in set (0.00 sec)
```

すべてのカラムを羅列して記述する代わりに`*`を使用できます。

```sql
mysql> SELECT *
    -> FROM carts;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | taro      | 2021-05-09 02:14:07 |
|  2 | hanako    | 2021-05-09 02:14:07 |
|  3 | jiro      | 2021-05-09 02:16:12 |
|  4 | saburo    | 2021-05-09 02:16:12 |
|  5 | shiro     | 2021-05-09 02:16:12 |
|  6 | goro      | 2021-05-09 02:16:12 |
+----+-----------+---------------------+
6 rows in set (0.00 sec)
```

### WHERE

すべてのレコードを取得するのではなく、条件を指定してレコードを取得してみましょう。

`user_name` が`taro`のレコードだけ取得します。

`WHERE`というキーワードを使用します。

```sql
mysql> SELECT *
    -> FROM carts
    -> WHERE user_name = 'taro';
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | taro      | 2021-05-09 02:14:07 |
+----+-----------+---------------------+
1 row in set (0.00 sec)
```

`=`は文字列や整数型の条件に対して使用します。

全条件はこちらを参照してください。

[演算子](https://dev.mysql.com/doc/refman/5.6/ja/non-typed-operators.html)

条件は複数指定できます。 最初の条件を`AND`でつなげます。

```sql
mysql> SELECT *
    -> FROM carts
    -> WHERE user_name = 'taro'
    ->   AND id = 1;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | taro      | 2021-05-09 02:14:07 |
+----+-----------+---------------------+
1 row in set (0.00 sec)
```

```sql
WHERE 条件 AND 条件 [AND 条件...]
```

### ORDER BY

MySQLは`SELECT`時のデータの並び順は何も保証せず返却します。

設定すれば、並び順を指定して値を取得できます。

では`id`の大きい順(降順)にデータを取得してみます。

```sql
mysql> SELECT *
    -> FROM carts
    -> ORDER BY id DESC;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  6 | goro      | 2021-05-09 02:16:12 |
|  5 | shiro     | 2021-05-09 02:16:12 |
|  4 | saburo    | 2021-05-09 02:16:12 |
|  3 | jiro      | 2021-05-09 02:16:12 |
|  2 | hanako    | 2021-05-09 02:14:07 |
|  1 | taro      | 2021-05-09 02:14:07 |
+----+-----------+---------------------+
6 rows in set (0.00 sec)
```

```sql
ORDER BY カラム名 [条件]
```

`DESC`とは降順(Descendant)を示します。

昇順にしたい場合は`ASC`を指定します。

```sql
mysql> SELECT *
    -> FROM carts
    -> ORDER BY id ASC;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | taro      | 2021-05-09 02:14:07 |
|  2 | hanako    | 2021-05-09 02:14:07 |
|  3 | jiro      | 2021-05-09 02:16:12 |
|  4 | saburo    | 2021-05-09 02:16:12 |
|  5 | shiro     | 2021-05-09 02:16:12 |
|  6 | goro      | 2021-05-09 02:16:12 |
+----+-----------+---------------------+
6 rows in set (0.00 sec)
```

## レコードの更新

[SQL](https://github.com/burning-tech/rdbms-training/blob/main/src/update_record.sql)

データベースに保存されているデータを更新してみましょう。

`UPDATE`を使用します。

```sql
mysql> UPDATE carts
    -> SET user_name = 'john';
Query OK, 6 rows affected (0.00 sec)
Rows matched: 6  Changed: 6  Warnings: 0
```

```sql
UPDATE テーブル名 SET カラム名 = 値;
```

データを取得してみます。

```sql
mysql> SELECT *
    -> FROM carts;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | john      | 2021-05-09 02:14:07 |
|  2 | john      | 2021-05-09 02:14:07 |
|  3 | john      | 2021-05-09 02:16:12 |
|  4 | john      | 2021-05-09 02:16:12 |
|  5 | john      | 2021-05-09 02:16:12 |
|  6 | john      | 2021-05-09 02:16:12 |
+----+-----------+---------------------+
6 rows in set (0.00 sec)
```

すべてのレコードの`user_name`が`john`になってしまいました。

特定のデータのみを更新する場合は`SELECT`同様に`WHERE`を使用します。

```sql
mysql> UPDATE carts
    -> SET user_name = 'ken'
    -> WHERE id = 1;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

データを再度取得してみましょう。

```sql
mysql> SELECT *
    -> FROM carts;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | ken       | 2021-05-09 02:14:07 |
|  2 | john      | 2021-05-09 02:14:07 |
|  3 | john      | 2021-05-09 02:16:12 |
|  4 | john      | 2021-05-09 02:16:12 |
|  5 | john      | 2021-05-09 02:16:12 |
|  6 | john      | 2021-05-09 02:16:12 |
+----+-----------+---------------------+
6 rows in set (0.00 sec)
```

id = 1のデータのみを更新できました。

複数のフィールドを一度に更新できます。

```sql
mysql> UPDATE carts
    -> SET user_name = 'tyler',
    ->     start_at = CURRENT_TIMESTAMP
    -> WHERE id = 1;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

データを再度取得してみましょう。

```sql
mysql> SELECT *
    -> FROM carts;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | tyler     | 2021-05-09 03:04:23 |
|  2 | john      | 2021-05-09 02:14:07 |
|  3 | john      | 2021-05-09 02:16:12 |
|  4 | john      | 2021-05-09 02:16:12 |
|  5 | john      | 2021-05-09 02:16:12 |
|  6 | john      | 2021-05-09 02:16:12 |
+----+-----------+---------------------+
6 rows in set (0.00 sec)
```

`user_name`も`start_at`も更新されています。

## レコードの削除

[SQL](https://github.com/burning-tech/rdbms-training/blob/main/src/delete_record.sql)

レコードの削除は`DELETE FROM`で行います。

`user_name`が`john`以外のレコードを削除してみましょう。

`DELTE FROM`にも`WHERE`が使えます。

```sql
mysql> DELETE FROM carts WHERE user_name <> 'tyler';
Query OK, 5 rows affected (0.00 sec)
```

```sql
DELETE FROM テーブル名
```

データを取得してみましょう。

```sql
mysql> SELECT * FROM carts;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | tyler     | 2021-05-09 03:04:23 |
+----+-----------+---------------------+
1 row in set (0.00 sec)
```

レコードが一つだけになっています。

`WHERE`の条件は`<>`を使うことで否定を表現できます。

## リレーション

[SQL](https://github.com/burning-tech/rdbms-training/blob/main/src/join.sql)

### FOREIGN KEY

exampleデータで見たようにテーブル同士をつなげることができます。

まず、cartsテーブルを見直してみます。

```sql
mysql> desc carts;
+-----------+--------------+------+-----+-------------------+----------------+
| Field     | Type         | Null | Key | Default           | Extra          |
+-----------+--------------+------+-----+-------------------+----------------+
| id        | int(11)      | NO   | PRI | NULL              | auto_increment |
| user_name | varchar(255) | NO   |     | NULL              |                |
| start_at  | datetime     | NO   |     | CURRENT_TIMESTAMP |                |
+-----------+--------------+------+-----+-------------------+----------------+
3 rows in set (0.00 sec)
```

このcartテーブルはユーザーの名前を格納するためのuser_nameフィールドを持ちます。

もし同じユーザーのデータが別日にこのテーブルに格納されてきたとします。

下記を実行してみてください。

```sql
mysql> INSERT INTO carts (user_name) VALUES
    ->   ('tatsuro'),
    ->   ('tatsuro');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql>
mysql> SELECT *
    -> FROM carts;
+----+-----------+---------------------+
| id | user_name | start_at            |
+----+-----------+---------------------+
|  1 | tyler     | 2021-05-09 03:04:23 |
|  7 | tatsuro   | 2021-05-09 03:20:35 |
|  8 | tatsuro   | 2021-05-09 03:20:35 |
+----+-----------+---------------------+
3 rows in set (0.00 sec)
```

id 7と8のデータが同一人物なのか別人なのかわたしたちは判別できません。

そこでユーザーを示すテーブルを作成します。

```sql
mysql> CREATE TABLE users (
    ->   id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    ->   first_name VARCHAR(255) NOT NULL,
    ->   last_name VARCHAR(255) NOT NULL
    -> );
Query OK, 0 rows affected (0.04 sec)
```

cartsテーブルも作り直しましょう。

テーブルを削除するには`DROP TABLE`を使用します。

```sql
mysql> DROP TABLE carts;
Query OK, 0 rows affected (0.02 sec)
```

新しい`carts`テーブルの定義は下記のようになります。

```sql
mysql> CREATE TABLE carts (
    ->   id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    ->   user_id INT NOT NULL,
    ->   start_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    ->   FOREIGN KEY (user_id) REFERENCES users (id)
    -> );
Query OK, 0 rows affected (0.05 sec)
```

違う部分から確認しましょう。

```sql
user_id INT NOT NULL,
```

`user_name`カラムが`user_id`という`INT`のカラムになっています。

```sql
FOREIGN KEY (user_id) REFERENCES users (id)
```

新しいキーワード`FOREIGN KEY`と`REFERENCES`が登場しました。

`FOREIGN KEY`とは外部キーといわれ、別のテーブルのカラムの値を参照することを宣言します。

`FOREIGN KEY (外部キーにしたいカラム名) REFERNECES 別のテーブル (外部キーが参照する別テーブルのカラム名)` と書きます。

`FOREIGN KEY (user_id) REFERENCES users (id)`の場合は、`user_id`カラムが`users`テーブルの`id`カラムを参照するという意味になります。

では、cartsテーブルとusersにデータを挿入します。

```sql
mysql> INSERT INTO users (first_name, last_name) VALUES
    ->   ('john', 'wick'),
    ->   ('peter', 'parker'),
    ->   ('bruce', 'wayne'),
    ->   ('luke', 'skywalker');
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0
```

```sql
mysql> INSERT INTO carts (user_id) VALUES
    ->   ((SELECT id FROM users WHERE first_name = 'john' AND last_name = 'wick')),
    ->   ((SELECT id FROM users WHERE first_name = 'john' AND last_name = 'wick')),
    ->   ((SELECT id FROM users WHERE first_name = 'john' AND last_name = 'wick')),
    ->   ((SELECT id FROM users WHERE first_name = 'peter' AND last_name = 'parker')),
    ->   ((SELECT id FROM users WHERE first_name = 'peter' AND last_name = 'parker')),
    ->   ((SELECT id FROM users WHERE first_name = 'peter' AND last_name = 'parker')),
    ->   ((SELECT id FROM users WHERE first_name = 'peter' AND last_name = 'parker')),
    ->   ((SELECT id FROM users WHERE first_name = 'bruce' AND last_name = 'wayne')),
    ->   ((SELECT id FROM users WHERE first_name = 'bruce' AND last_name = 'wayne'));
Query OK, 9 rows affected (0.01 sec)
Records: 9  Duplicates: 0  Warnings: 0
```

上記のように`INSERT`で入れるデータとしてSELECTの結果を使うことができます。

ここで詳細には踏み込みませんが、これを**サブクエリ**ということを頭の片隅に置いておいてください。

結果を確認しておきましょう。

```sql
mysql> SELECT *
    -> FROM users;
+----+------------+-----------+
| id | first_name | last_name |
+----+------------+-----------+
|  1 | john       | wick      |
|  2 | peter      | parker    |
|  3 | bruce      | wayne     |
|  4 | luke       | skywalker |
+----+------------+-----------+
4 rows in set (0.00 sec)

mysql>
mysql> SELECT *
    -> FROM carts;
+----+---------+---------------------+
| id | user_id | start_at            |
+----+---------+---------------------+
|  1 |       1 | 2021-05-09 03:51:49 |
|  2 |       1 | 2021-05-09 03:51:49 |
|  3 |       1 | 2021-05-09 03:51:49 |
|  4 |       2 | 2021-05-09 03:51:49 |
|  5 |       2 | 2021-05-09 03:51:49 |
|  6 |       2 | 2021-05-09 03:51:49 |
|  7 |       2 | 2021-05-09 03:51:49 |
|  8 |       3 | 2021-05-09 03:51:49 |
|  9 |       3 | 2021-05-09 03:51:49 |
+----+---------+---------------------+
9 rows in set (0.00 sec)
```

### JOIN

では上記のcartsテーブルとusersテーブルのデータを結合してみましょう。

`SELECT`と一緒に`JOIN`を使用します。

```sql
mysql> SELECT carts.id,
    ->        carts.start_at,
    ->        users.first_name,
    ->        users.last_name
    -> FROM carts
    -> JOIN users ON users.id = carts.user_id;
+----+---------------------+------------+-----------+
| id | start_at            | first_name | last_name |
+----+---------------------+------------+-----------+
|  1 | 2021-05-09 03:51:49 | john       | wick      |
|  2 | 2021-05-09 03:51:49 | john       | wick      |
|  3 | 2021-05-09 03:51:49 | john       | wick      |
|  4 | 2021-05-09 03:51:49 | peter      | parker    |
|  5 | 2021-05-09 03:51:49 | peter      | parker    |
|  6 | 2021-05-09 03:51:49 | peter      | parker    |
|  7 | 2021-05-09 03:51:49 | peter      | parker    |
|  8 | 2021-05-09 03:51:49 | bruce      | wayne     |
|  9 | 2021-05-09 03:51:49 | bruce      | wayne     |
+----+---------------------+------------+-----------+
9 rows in set (0.00 sec)
```

```sql
SELECT カラム名 FROM テーブルA JOIN テーブルB ON テーブルA.カラム名 = テーブルB.カラム名
```

`WHERE`と併用もできます。

```sql
mysql> SELECT carts.id,
    ->        carts.start_at,
    ->        users.first_name,
    ->        users.last_name
    -> FROM carts
    -> JOIN users ON users.id = carts.user_id
    -> WHERE first_name = 'john';
+----+---------------------+------------+-----------+
| id | start_at            | first_name | last_name |
+----+---------------------+------------+-----------+
|  1 | 2021-05-09 03:51:49 | john       | wick      |
|  2 | 2021-05-09 03:51:49 | john       | wick      |
|  3 | 2021-05-09 03:51:49 | john       | wick      |
+----+---------------------+------------+-----------+
3 rows in set (0.00 sec)
```

carts.idというように書いているのidフィールドはusersテーブルにもcartsテーブルにもあるのでどちらかSQLが識別できず、エラーになるためです。

```sql
mysql> SELECT id
    -> FROM carts
    -> JOIN users ON users.id = carts.user_id;
ERROR 1052 (23000): Column 'id' in field list is ambiguous
```

ちなみにテーブル名には`AS`で別名をつけて、SQLを短くできます。

```sql
mysql> SELECT c.id,
    ->        c.start_at,
    ->        u.first_name,
    ->        u.last_name
    -> FROM carts AS c
    -> JOIN users AS u ON u.id = c.user_id;
+----+---------------------+------------+-----------+
| id | start_at            | first_name | last_name |
+----+---------------------+------------+-----------+
|  1 | 2021-05-09 03:51:49 | john       | wick      |
|  2 | 2021-05-09 03:51:49 | john       | wick      |
|  3 | 2021-05-09 03:51:49 | john       | wick      |
|  4 | 2021-05-09 03:51:49 | peter      | parker    |
|  5 | 2021-05-09 03:51:49 | peter      | parker    |
|  6 | 2021-05-09 03:51:49 | peter      | parker    |
|  7 | 2021-05-09 03:51:49 | peter      | parker    |
|  8 | 2021-05-09 03:51:49 | bruce      | wayne     |
|  9 | 2021-05-09 03:51:49 | bruce      | wayne     |
+----+---------------------+------------+-----------+
9 rows in set (0.00 sec)
```

`AS`は省略できます。

```sql
mysql> SELECT c.id,
    ->        c.start_at,
    ->        u.first_name,
    ->        u.last_name
    -> FROM carts c
    -> JOIN users u ON u.id = c.user_id;
+----+---------------------+------------+-----------+
| id | start_at            | first_name | last_name |
+----+---------------------+------------+-----------+
|  1 | 2021-05-09 03:51:49 | john       | wick      |
|  2 | 2021-05-09 03:51:49 | john       | wick      |
|  3 | 2021-05-09 03:51:49 | john       | wick      |
|  4 | 2021-05-09 03:51:49 | peter      | parker    |
|  5 | 2021-05-09 03:51:49 | peter      | parker    |
|  6 | 2021-05-09 03:51:49 | peter      | parker    |
|  7 | 2021-05-09 03:51:49 | peter      | parker    |
|  8 | 2021-05-09 03:51:49 | bruce      | wayne     |
|  9 | 2021-05-09 03:51:49 | bruce      | wayne     |
+----+---------------------+------------+-----------+
9 rows in set (0.00 sec)
```

`SELECT`で選択するフィールド名を`AS`で変更できます。

`AS`は省略できます。

```sql
mysql> SELECT c.id AS cart_id,
    ->        c.start_at AS cart_start_at,
    ->        u.first_name AS user_first_name,
    ->        u.last_name AS user_last_name
    -> FROM carts c
    -> JOIN users u ON u.id = c.user_id;
+---------+---------------------+-----------------+----------------+
| cart_id | cart_start_at       | user_first_name | user_last_name |
+---------+---------------------+-----------------+----------------+
|       1 | 2021-05-09 03:51:49 | john            | wick           |
|       2 | 2021-05-09 03:51:49 | john            | wick           |
|       3 | 2021-05-09 03:51:49 | john            | wick           |
|       4 | 2021-05-09 03:51:49 | peter           | parker         |
|       5 | 2021-05-09 03:51:49 | peter           | parker         |
|       6 | 2021-05-09 03:51:49 | peter           | parker         |
|       7 | 2021-05-09 03:51:49 | peter           | parker         |
|       8 | 2021-05-09 03:51:49 | bruce           | wayne          |
|       9 | 2021-05-09 03:51:49 | bruce           | wayne          |
+---------+---------------------+-----------------+----------------+
9 rows in set (0.00 sec)
```

### LEFT JOIN

ではそれぞれのユーザーが何時頃に買い物をしているのかを知りたいとします。

ユーザーとカートの一覧を表示しましょう。

```sql
mysql> SELECT u.first_name,
    ->        u.last_name,
    ->        c.start_at
    -> FROM users u
    -> JOIN carts c ON u.id = c.user_id;
+------------+-----------+---------------------+
| first_name | last_name | start_at            |
+------------+-----------+---------------------+
| john       | wick      | 2021-05-09 03:51:49 |
| john       | wick      | 2021-05-09 03:51:49 |
| john       | wick      | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| bruce      | wayne     | 2021-05-09 03:51:49 |
| bruce      | wayne     | 2021-05-09 03:51:49 |
+------------+-----------+---------------------+
9 rows in set (0.00 sec)
```

買い物をしたことがあるユーザーはここに表示されました。しかし、買い物をしていない`luke`はここに表示されません。

買い物をしていないユーザーはNULL(データがない)として表示したいとします。

その場合は`LEFT JOIN`を使います。

```sql
mysql> SELECT u.first_name,
    ->        u.last_name,
    ->        c.start_at
    -> FROM users u
    -> LEFT JOIN carts c ON u.id = c.user_id;
+------------+-----------+---------------------+
| first_name | last_name | start_at            |
+------------+-----------+---------------------+
| john       | wick      | 2021-05-09 03:51:49 |
| john       | wick      | 2021-05-09 03:51:49 |
| john       | wick      | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| peter      | parker    | 2021-05-09 03:51:49 |
| bruce      | wayne     | 2021-05-09 03:51:49 |
| bruce      | wayne     | 2021-05-09 03:51:49 |
| luke       | skywalker | NULL                |
+------------+-----------+---------------------+
10 rows in set (0.00 sec)
```

`LEFT JOIN`は`LEFT OUTER JOIN`の省略形です。

左外部結合といいます。

外部結合とは主テーブル(ここではusers)のレコードをすべて表示し、外部結合のテーブル(ここではcarts)のレコードがあれば、結合して表示します。

外部結合は詰まる人がかなりいるポイントなので下記のURL等を参考にしてみてください。

<http://www.sql-reference.com/select/outer_join.html>

先に説明をした`LEFT JOIN`ではなく、`JOIN`は`INNER JOIN`(内部結合)の略なので覚えておいてください。

## まとめ

データベースがどんなものでどのように使うのかはなんとなくわかったと思います。

今はそれで十分です。 これ以降の章でも不足したデータベースの知識を少しずつ拡充していきます。

しかし、長期的にはより深い知識の習得をおすすめしますので、Appendixや参考文献を読んで、データベースについて肌感覚を掴んでいってください。
