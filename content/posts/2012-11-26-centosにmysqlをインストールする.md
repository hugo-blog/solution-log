---
title: CentOS6に最新バージョンのMySQLとphpMyAdminをインストールする
author: tomonori
type: post
date: 2012-11-25T17:04:21+00:00
url: /?p=187
categories:
  - CentOS6
  - MySQL
  - phpMyAdmin
tags:
  - Remi

---
yumからのインストールだと、MySQL5.1系がインストールされ、どうやらこれがPHP5.4.9と親和性が取れないらしく、phpMyAdminでエラーが出てしまう。なのでMySQL5.5系をインストールする。

### MySQL5.5系をインストールする

5.1系が既にインストールされている場合、最初に5.1系を削除。その後Remiリポジトリから5.5系をインストールする。**事前に[Remiリポジトリが追加されているか確認][1]しておく事**。

```:bash
# MySQLを停止する
/etc/rc.d/init.d/mysqld stop
# MySQLを削除する
yum erase mysql mysql-server mysql-devel
# 5.5系をインストール
yum --enablerepo=remi install mysql mysql-server mysql-devel
```

### MySQLの初期設定を行う

MySQLを初めて起動する場合、ユーザー名やパスワードなどの設定を行う必要がある

```:bash
/etc/rc.d/init.d/mysqld start
mysql_secure_installation
```

このあと適宜ユーザー名やパスワードを設定する

### MySQLのバージョンを確認する

一応ここでバージョンを確認しておく

```:bash
mysql --version
mysql  Ver 14.14 Distrib 5.5.29, for Linux (x86_64) using readline 5.1
```

### my.confを設定する

[こちらのサイト][2]を参考にしてみた。**『/etc/my.cnf』**を以下のように修正する

```:text
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
character-set-server=utf8

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

[mysql]
default-character-set=utf8
```

### phpMyAdminをインストールする

基本的には[phpMyAdminの公式サイト][3]からzip版をダウンロードして任意のフォルダに解凍する。svn版はメンテナンスが止まっているみたいなので使わない。phpMyAdminは**MySQLの初期設定が終わっていないと立ち上がらないので注意**。
  
またダウンロードしたアーカイブをscpでサーバにアップする場合は

```:bash
cd /path/to/phpMyAdmin-3.5.5-all-languages.tar.gz
scp phpMyAdmin-3.5.5-all-languages.tar.gz user@uri-adress:/var/www/html
```

もしくは[phpMyAdmin &#8211; Browse /phpMyAdmin at SourceForge.net][4]からでも取得出きるのでwgetで行ける。

phpMyAdminのアーカイブを展開する場合は

```:bash
tar xfvz phpMyAdmin-3.5.5-all-languages.tar.gz 
mv phpMyAdmin-3.5.5-all-languages phpmyadmin
```

### phpMyAdminの設定をする

初期設定のままだと**「phpMyAdmin の設定保存場所が完全に設定されていないため、いくつかの拡張機能が無効になっています。理由についてはこちらをご覧ください。」**と言う警告が出るので設定をいじる。

#### データベースとテーブルを作成する

```:bash
mysql -u root -p
# データベースphpmyadminとテーブルを作成する
mysql&gt; source /var/www/html/phpmyadmin/examples/create_tables.sql; 
GRANT ALL ON phpmyadmin.* TO pma@localhost IDENTIFIED BY "パスワード";
```

#### phpMyAdminの設定ファイルを設定する

```:bash
cd /var/www/html/phpmyadmin
cp config.sample.inc.php config.inc.php
vi config.inc.php
```

以下の設定で運用するようにする。他の箇所はコメントアウトしない。**pmaのパスワード変更を忘れずに行う**。

```:php
/*
 * phpMyAdmin configuration storage settings.
 */

/* User used to manipulate with storage */
$cfg["Servers"][$i]["controlhost"] = "";
$cfg["Servers"][$i]["controluser"] = "pma";
$cfg["Servers"][$i]["controlpass"] = "パスワード"; #ディフォルトだと危険なので変更をかけておく

/* Storage database and tables */
$cfg["Servers"][$i]["pmadb"] = "phpmyadmin";
$cfg["Servers"][$i]["bookmarktable"] = "pma_bookmark";
$cfg["Servers"][$i]["relation"] = "pma_relation";
$cfg["Servers"][$i]["table_info"] = "pma_table_info";
$cfg["Servers"][$i]["table_coords"] = "pma_table_coords";
$cfg["Servers"][$i]["pdf_pages"] = "pma_pdf_pages";
$cfg["Servers"][$i]["column_info"] = "pma_column_info";
$cfg["Servers"][$i]["history"] = "pma_history";
$cfg["Servers"][$i]["table_uiprefs"] = "pma_table_uiprefs";
$cfg["Servers"][$i]["tracking"] = "pma_tracking";
$cfg["Servers"][$i]["designer_coords"] = "pma_designer_coords";
$cfg["Servers"][$i]["userconfig"] = "pma_userconfig";
$cfg["Servers"][$i]["recent"] = "pma_recent";
/* Contrib / Swekey authentication */
$cfg["Servers"][$i]["auth_swekey_config"] = "/etc/swekey-pma.conf";
```

#### 設定を反映させる

phpMyAdminからログアウト。**ブラウザのキャッシュをクリア**して再びログインする。ブラウザのキャッシュがクリアされないとエラー表示がいつまでも消えないので注意。

### php-mcryptをインストールする

**「mcrypt 拡張がありません。PHP の設定をチェックしてみてください。」**と言う警告が出ている場合はphp-mcryptをインストールする。**事前に[Remiリポジトリが追加されているか確認][1]しておく事**。mcryptインストール後はMySQLとApacheを再起動する。

```:bash
yum -y install --enablerepo=remi php-mcrypt
service mysqld restart
service httpd restart
```

### MySQLの自動起動設定をする

```:bash
chkconfig mysqld on
```

### 参考サイト

  * [MySQL 5.0 から 5.5 へアップデート &#8211; takaya030の備忘録][5] 
      * [CentOS 6上でRedmine 2を動かすメモ][2] 
          * [MySQL用GUI設定ツール導入(phpMyAdmin) &#8211; CentOSで自宅サーバー構築][6] 
              * [phpMyAdmin][7] 
                  * [「phpMyAdmin の設定保存場所が完全に設定されていないため～」の解消 &#8211; やすはるラボ][8] 
                      * [centos6+phpmyadminで「mcrypt 拡張がありません。PHP の設定をチェックしてみてください。」 &#8211; [Swb:]渋谷に住むWEBデザイナの備忘録][9] </ul>

 [1]: ./?p=542
 [2]: http://www.02.246.ne.jp/~torutk/swetools/redmine/setupCentOS6.html#SEC19
 [3]: http://www.phpmyadmin.net
 [4]: http://sourceforge.net/projects/phpmyadmin/files/phpMyAdmin/
 [5]: http://d.hatena.ne.jp/takaya030/20120630/1341072325
 [6]: http://centossrv.com/phpmyadmin.shtml
 [7]: http://www.phpmyadmin.ne
 [8]: http://d.hatena.ne.jp/yasuhallabo/20111204/1323015635
 [9]: http://d.hatena.ne.jp/susan-style/20110929/1317289484