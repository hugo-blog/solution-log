---
title: CentOS6.3にRedmine環境を構築する
author: tomonori
type: post
date: 2012-11-26T17:13:25+00:00
url: /?p=190
categories:
  - CentOS6
  - Redmine
tags:
  - apache
  - mysql
  - ruby

---
Tracがどうも使いにくいのでRedmineに移行してみる実験をする。

### CentOSパッケージの追加インストール

まずはEPELが導入されているか確かめる。導入されてなければ[インストールする][1]。

yumで追加インストール

```:bash
# 開発パッケージを導入。こちらは使わないものもいくらか入る
yum groupinstall &quot;Development Tools&quot;
# 通信関係を入れたりアップデートする
yum install openssl-devel readline-devel zlib-devel curl-devel libyaml-devel
# MySQLをアップデートする（既に導入済み）Remiリポジトリ経由ならRemiリポジトリを使う事！
yum install mysql-server mysql-devel
# Apacheをアップデートする（一応やっておく）
yum install httpd httpd-devel
# ImageMagickをインストールアップデートする（多分入ってない）
yum install ImageMagick ImageMagick-devel
```

### SELinuxを無効にする

エディタで /etc/sysconfig/selinux を開き、 SELINUX の値を disabled に編集する。変更後はシステムを再起動しておくこと。

```:bash
vi /etc/sysconfig/selinux
SELINUX=disabled
reboot
```

### iptablesでHTTPを許可する

CentOS 6.4の初期状態ではiptables(ファイアウォール)が有効になっており、外部からサーバ上の80/tcpポート(HTTP)に接続することができない。クライアントのwebブラウザからアクセスできるようiptablesの設定を変更する。

```:bash
vi /etc/sysconfig/iptables

# Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT

# iptablesを再起動する
service iptables restart
```

### Ruby 1.9.3をインストールする

既にRuby 2.0.0がリリースされているが、Passengerのリリース版がRuby 2.0.0では動作しないため、Ruby 1.9.3を使用する。

```:bash
cd /usr/local/src
wget ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p392.tar.gz
tar zxvf ruby-1.9.3-p392.tar.gz
cd ruby-1.9.3-p392
./configure --prefix=/usr/local --enable-shared --disable-install-doc --with-opt-dir=/usr/local/lib
make
make install
```

### データベースの設定を確認する

**MySQLのバージョンが古い場合は必ず[アップデートしておく][2]事。**

『**/etc/my.cnf**』を開き、**[mysqld]**セクションに**character-set-server=utf8**を、**[mysql]**セクションに**default-character-set=utf8**を設定する

```:bash
vi /etc/my.cnf
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

character-set-server=utf8

# 任意設定
innodb_file_per_table
query-cache-size=16M

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

[mysql]
default-character-set=utf8

#mysqlを再起動する
service mysqld start
chkconfig mysqld on
```

### Redmineをインストールする

Subversionリポジトリからチェックアウトするかソースからインストールする。Subversionリポジトリからチェックアウトした場合はファイルの解凍を行う必要がない。どちらかと言えばSubversionリポジトリからチェックアウトした方が簡単だと思われる。またダウンロードした[Redmineのバージョン確認][3]を行うと良い。

```:bash
cd /var/lib
svn checkout http://svn.redmine.org/redmine/branches/2.2-stable redmine
```

もしくは

```:bash
cd /usr/local/src
wget http://rubyforge.org/frs/download.php/76627/redmine-2.2.0.tar.gz
cd /var/lib
tar xzf /usr/local/src/redmine-2.2.0.tar.gz
ln -s redmine-2.2.0 redmine
ls -l
```

### Redmineの設定ファイルを作成する

設定ファイルはシンボリックリンクではなく、Redmine本体に作成する事。

#### データベース接続設定

**『/path/to/redmine/config/database.yml』**を作成。

```:bash
production:
      adapter: mysql2
      database: redmine
      host: localhost
      username: redmine
      password: foobar
      encoding: utf8  
```

下線部は、MySQLの設定で使用した名前に合わせます。

#### メール送信設定

**『/path/to/redmine/config/configuration.yml』**を作成。メール送信の設定は、環境によって異なる。以下は認証不要のSMPTサーバーの場合。

```:bash
production:
      email_delivery:
        delivery_method: :smtp
        smtp_settings:
          address: "localhost"
          port: 25
          domain: "example.com"
```

### bundlerをインストールする

```:bash
gem install bundler --no-rdoc --no-ri
```

### Redmineが使用するgemパッケージのインストールする

Rubyのgemとは別にインストールする必要があるらしい。

```:bash
cd /var/lib/redmine-2.2.0
bundle install --path vendor/bundler --without development test postgresql sqlite
```

もしこの時以下のようなメッセージが出た場合は[mysql-develがインストールされていない][4]ので[インストールする][2]事

```:bash
An error occurred while installing mysql2 (0.3.11), and Bundler cannot continue.
Make sure that `gem install mysql2 -v "0.3.11"` succeeds before bundling.
```

### Redmineを初期化する

まずデータベースredmineを作成する。その後データベースユーザredmineを登録する。

#### データベースRedmineの作成

**必ずキャラクタセットを指定してデータバースを作成する事！**この設定が曖昧だと、バックアップや移設作業の時に困る事になる。

```:bash
mysql -u root -p
mysql&gt; CREATE DATABASE redmine CHARACTER SET utf8;
```

#### 登録ユーザの確認と作成

```:bash
mysql -u root -p
Enter password: 

mysql&gt; SELECT User, Host FROM mysql.user;
+------+-----------+
| User | Host      |
+------+-----------+
| root | 127.0.0.1 |
| root | localhost |
+------+-----------+
```

現在登録されているユーザ一覧にredmineが無い場合はユーザを追加（**パスワードは/path/to/redmine/config/database.ymlで設定したものと同じものにする**）

```:bash
mysql&gt; GRANT ALL PRIVILEGES ON redmine.* TO "redmine"@"localhost" IDENTIFIED BY "パスワード";
Query OK, 0 rows affected (0.00 sec)

# redmineがユーザとして追加されたか確認
mysql&gt; SELECT User, Host FROM mysql.user;
+--------------+-----------+
| User         | Host      |
+--------------+-----------+
| root         | 127.0.0.1 |
| redmine      | localhost |
| root         | localhost |
+--------------+-----------+
3 rows in set (0.00 sec)
```

間違えてユーザを作成してしまった場合は**『DROP USER redmine@localhost』**などで削除出来る。

#### セッションデータの暗号化の鍵生成

```:bash
cd /var/lib/redmine-2.2.0
bundle exec rake generate_secret_token
```

#### データベースを初期構築

データベースの初期構築コマンドを発行する前にMySQLの起動を忘れずに行う。

```:bash
service mysqld start
RAILS_ENV=production bundle exec rake db:migrate
```

もし**Access denied for user &#8216;redmine&#8217;@&#8217;localhost&#8217; (using password: YES)&#8230;**のようなエラーが出てデータベースの初期化が進まない時はもう一度config/database.ymlのユーザとパスワードの記述が正しいかどうか確かめる。もしくはymlファイル通りにMySQLでユーザに権限を与えたかどうか調べる。

また**rake aborted!Please install the mysql adapter: \`gem install activerecord-mysql-adapter\` (mysql is not part of the bundle. Add it to Gemfile.)**や**rake aborted!
  
database configuration does not specify adapter**のようなエラーが出る場合は、database.ymlのアダプターの設定が**mysql2**になっているか再度確認する。

### Redmineの動作確認をする

上記の手順通りRedmineを設定したら、とりあえず起動してみる。

```:bash
# Apacheを起動する
httpd -k start
# WEBrick起動する
ruby /var/lib/redmine/script/rails server webrick -e production
```

<http://localhost:3000>に接続し、Redmine画面が表示されたら正常。

### RedmineをAapcheと同期させる

通常はコマンド操作でRedmineの立ち上げが必要になるが、一々面倒なので、Apache起動時にRedmineも立ち上がるように設定する。[こちらのエントリー][5]を参考。

### 参考サイト

  * [CentOS 6上でRedmine 2を動かすメモ][6] 
      * [CentOS 外部レポジトリの追加(EPEL)][7] 
          * [mysql &#8211; Errors Installing mysql2 gem via the Bundler &#8211; Stack Overflow][4] </ul>

 [1]: ./?p=548
 [2]: ./?p=187
 [3]: ./?p=596
 [4]: http://stackoverflow.com/questions/3754662/errors-installing-mysql2-gem-via-the-bundler
 [5]: ./?p=459
 [6]: http://www.02.246.ne.jp/~torutk/swetools/redmine/setupCentOS6.html
 [7]: http://www.tooyama.org/yum-addrepo-epel.html