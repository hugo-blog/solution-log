---
title: Redmine2.4をCentOS6.4にインストールする
author: tomonori
type: post
date: 2014-01-01T17:42:18+00:00
url: /?p=914
categories:
  - Redmine

---
Redmine、CentOSのバージョンが上がったのでインストール手順を再確認する。

### SELinuxを無効にする

```:bash
vi /etc/sysconfig/selinux
SELINUX=disabled
reboot
```

### iptablesでHTTPを許可する

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

# Redmineの動作確認用に3000番を開けておく
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3000 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT

# iptablesを再起動する
service iptables restart
```

### CentOSパッケージの追加インストール

yumで追加インストール

```:bash
# EPELをインストール
rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

# 開発ツールをインストール
yum groupinstall "Development Tools";
# RubyとPassengerのビルドに必要なヘッダファイルなど
yum install openssl-devel readline-devel zlib-devel curl-devel libyaml-devel
# MySQLをアップデートする
yum install mysql-server mysql-devel
# Apacheをアップデートする
yum install httpd httpd-devel
# ImageMagickとヘッダファイル・日本語フォント
yum install ImageMagick ImageMagick-devel
yum install ipa-pgothic-fonts
```

### Ruby 2.0.0をインストールする

```:bash
cd /usr/local/src
wget ftp://ftp.ruby-lang.org/pub/ruby/2.0/ruby-2.0.0-p353.tar.gz
tar zxvf ruby-2.0.0-p353.tar.gz
cd ruby-2.0.0-p353
./configure --disable-install-doc
make
make install
```

### bundlerをインストールする

```:bash
gem install bundler --no-rdoc --no-ri
```

### データベースの設定を確認する

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

#### /etc/my.cnf への設定が反映されていることの確認

```:bash
mysql -uroot
mysql&gt; show variables like "character_set%";
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)

mysql&gt; exit
```

#### rootユーザーのパスワード変更・匿名ユーザー削除

```:bash
mysql -uroot
mysql&gt; use mysql;
mysql&gt; update user set password=password("********") where user = "root";
mysql&gt; delete from user where user = "";
mysql&gt; flush privileges;
mysql&gt; exit;
```

#### Redmine用データベースとユーザーを作成する

```:bash
mysql -uroot -p
mysql&gt; create database redmine default character set utf8;
mysql&gt; GRANT ALL PRIVILEGES ON redmine.* TO "redmine"@"localhost&#039 IDENTIFIED BY "パスワード";
mysql&gt; flush privileges;
mysql&gt; exit;
```

### Redmineをインストールする

```:bash
cd /var/lib
svn checkout http://svn.redmine.org/redmine/branches/2.4-stable redmine
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

### Redmineが使用するgemパッケージのインストールする

Rubyのgemとは別にインストールする必要があるらしい。

```:bash
cd /var/lib/redmine
bundle install --without development test
```

### Redmineを初期化する

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

### Redmineの動作確認をする

上記の手順通りRedmineを設定したら、とりあえず起動してみる。

```:bash
# Apacheを起動する
httpd -k start
# WEBrick起動する
ruby /var/lib/redmine/script/rails server webrick -e production
```

<http://localhost:3000>に接続し、Redmine画面が表示されたら正常。

### Passengerをインストールする

Apache上でRedmineなどのRailsアプリケーションの実行に必要なPhusion Passengerをインストールする

```:bash
gem install passenger --no-rdoc --no-ri

passenger-install-apache2-module


The Apache 2 module was successfully installed.

Please edit your Apache configuration file, and add these lines:


#######################################################
#  /etc/httpd/conf.d/passenger.conf に書き込む
#######################################################
   LoadModule passenger_module /usr/local/lib/ruby/gems/1.9.1/gems/passenger-3.0.17/ext/apache2/mod_passenger.so
   PassengerRoot /usr/local/lib/ruby/gems/1.9.1/gems/passenger-3.0.17
   PassengerRuby /usr/local/bin/ruby
##########################################################

After you restart Apache, you are ready to deploy any number of Ruby on Rails
applications on Apache, without any further Ruby on Rails-specific
configuration!

Press ENTER to continue.
```

#### Apacheの設定を行う

```:bash
vi /etc/httpd/conf.d/passenger.conf


# Passengerの基本設定。
# passenger-install-apache2-module --snippet を実行して表示される設定を使用。
# 環境によって設定値が異なりますので以下の3行はそのまま転記しないでください。
#
LoadModule passenger_module /usr/local/lib/ruby/gems/1.9.1/gems/passenger-3.0.17/ext/apache2/mod_passenger.so
PassengerRoot /usr/local/lib/ruby/gems/1.9.1/gems/passenger-3.0.17
PassengerRuby /usr/local/bin/ruby

# Passengerが追加するHTTPヘッダを削除するための設定（任意）。
#
Header always unset "X-Powered-By"
Header always unset "X-Rack-Cache"
Header always unset "X-Content-Digest"
Header always unset "X-Runtime"

# 必要に応じてPassengerのチューニングのための設定を追加（任意）。
# 詳しくはPhusion Passenger users guide(http://www.modrails.com/documentation/Users%20guide%20Apache.html)をご覧ください。
PassengerMaxPoolSize 20
PassengerMaxInstancesPerApp 4
PassengerPoolIdleTime 3600
PassengerHighPerformance on
PassengerStatThrottleRate 10
PassengerSpawnMethod smart
RailsAppSpawnerIdleTime 86400

service httpd start
chkconfig httpd on
```

#### Apache上のPassengerでRedmineを実行するための設定をする

```:bash
chown -R apache:apache /var/lib/redmine

ln -s /var/lib/redmine/public /var/www/html/redmine

vi /etc/httpd/conf.d/passenger.conf

# 以下を追加
RackBaseURI /redmine

service httpd configtest
service httpd graceful
```

### 参考サイト

  * [Redmine 2.4をCentOS 6.4にインストールする手順 | Redmine.JP Blog][1] </ul>

 [1]: http://blog.redmine.jp/articles/2_4/installation_centos/