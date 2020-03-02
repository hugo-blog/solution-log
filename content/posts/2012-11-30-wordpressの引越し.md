---
title: WordPressの引越し
author: tomonori
type: post
date: 2012-11-30T13:05:11+00:00
url: /?p=199
categories:
  - WordPress

---
WordPressをIPアドレス規制をかけてアクセスさせるように設定しようと思い、.htaccessにアクセス可能IPアドレスを記述。しかし困った事に直前にWordPressで使われていた.htaccessファイルを上書きしてしまい、WordPressが制御不能になってしまった。仕方ないのでWordPressのインストールディレクトリを変更して再構築を試みる。

### 新しいWordPressをインストールする

ターミナルからインストール先のサーバに接続。

```:bash
cd /path/to/wordpressfolder
git clone https://github.com/WordPress/WordPress.git
mv WordPress blogfolder
```

### WordPressの初期設定を行う

WordPressのトップディレクトリにアクセスすると必要情報を入力するように促される。この時移行元のWordPressと移行先のWordPressで**ユーザー名、パスワード、メールアドレスを同一にしておく**こと。ユーザー名、パスワード、メールアドレス等の変更が必要な場合は、**移設完了後**に行う。

### データベースを移行する

移動元のWordPressのデータフォルダから**optionsを除いたテーブルを移動させる**。基本的にoptionsテーブルはWordPress毎の設定になるので、新規で立ち上げたWordPressに使用しても、設定は反映されない。またダンプしたSQLファイルの中に旧WordPressのURLが含まれる場合は、新WordPressのURLに書き換えておくこと!

#### 移設元サーバ側

```:bash
# 一覧でWordPress用のデータベースを確認する
SHOW DATABASES;

# 登録されているユーザを確認する
SELECT user, host FROM mysql.user;

# WordPress用データベースのユーザーを確認する
SHOW GRANTS for "user"@"host";

# MySQLデータをダンプする
mysqldump -u {user} --opt --password={xxxxxxxx} --add-drop-table {wp_database} wp_commentmeta wp_comments wp_links wp_postmeta wp_posts wp_terms wp_term_relationships wp_term_taxonomy wp_usermeta wp_users &gt; /tmp/wp_database.sql

# ベースとなるURLを書き換える
sed "s|http://{移設元サーバWordPressインストールディレクトリ}|http://{移設先サーバWordPressインストールディレクトリ}|g" /tmp/wp_database.sql &gt; /tmp/wp_database.sql2
```

#### 移設先サーバ側

```:bash
mysql -uroot -p
MariaDB [(none)]&gt; create database wp_database default character set utf8;
MariaDB [(none)]&gt; grant all on wp_user.* to wp_user@localhost identified by "********";
MariaDB [(none)]&gt; flush privileges;
MariaDB [(none)]&gt; exit;

mysql -u root -pXXXXXXXX wp_database &lt; wp_database.sql
```

### テーマを移行する

WordPressが完全に立ち上がった状態で旧サーバで使っていたテーマを「**wp-content/thmemes**」へコピーする。themesは上書きしない。もし上書きして新しいWordPressに古いWordPressのテーマがインストールされていない場合表示エラーになる。

### ファイルを移行する

旧サーバで使っていたファイルを「**wp-content/**」へコピーする。

### プラグインなどの移動

今回はちょっと不安があったので全部手動で行った。

### パーマメントリンク設定を変更する

タグやカテゴリが有効にならないので旧WordPressで使用していたものと同じにする