---
title: CentOS7へRuby2.3とRails4をインストールする
author: tomonori
type: post
date: 2016-01-18T14:22:30+00:00
url: /?p=1134
categories:
  - CentOS7
  - Rails
  - Ruby

---
Railsでウェブサイトを作る必要が出てきそうなのでRailsをさくらのVPS2GBへインストールしサイト構築の練習をする。

### Rubyをインストールする

CentOS7標準のRubyのバージョンが古いので、2.3系をRPMパッケージでインストールする。Rubyがどのようにインストールされるかは[specファイル](https://github.com/feedforce/ruby-rpm/blob/master/ruby.spec)を確認する。Rubyのビルドに必要なパッケージも確認出来る。

> Requires: readline ncurses gdbm glibc openssl libyaml libffi zlib
  
> BuildRequires: readline-devel ncurses-devel gdbm-devel glibc-devel gcc openssl-devel make libyaml-devel libffi-devel zlib-devel

```:bash
# EPELをインストールする
rpm -ivh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
rpm --import http://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7

# 必要なパッケージをインストールする
yum --enablerepo=epel -y install gdbm-devel libdb4-devel libffi-devel libyaml libyaml-devel ncurses-devel openssl-devel readline-devel tcl-devel


# Rubyをインストールする
#RPMを作成する
mkdir -p /root/rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS}

cd /root/rpmbuild/SOURCES
curl -O http://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.0.tar.gz

cd /root/rpmbuild/SPECS
curl -O https://raw.githubusercontent.com/feedforce/ruby-rpm/master/ruby.spec

rpmbuild -bb /root/rpmbuild/SPECS/ruby.spec


# Rubyをインストールする
rpm -Uvh /root/rpmbuild/RPMS/ruby-2.3.0-1.el7.centos.x86_64.rpm 


ruby -v
# rpm -Uvh ruby-2.3.0-1.el7.centos.x86_64.rpm 

gem -v
2.5.1
```

### Rails4をインストールする

```:bash
yum --enablerepo=epel -y install nodejs /
mariadb-devel

gem install bundler 
gem install rails --no-ri --no-rdoc
gem install mysql2 --no-ri --no-rdoc -- --with-mysql-config=/usr/bin/mysql_config 

rails -v
Rails 4.2.5
```

### Railsの動作確認を行う

```:bash
cd /var/www/html
rails new demo

# Passengerで起動させる時は
chown -R apache:apache demo


# サーバのIPアドレスを調べる
ip addr show eth0 | grep inet | awk "{ print $2; }" | sed "s/\/.*$//"
rails server --binding=IPアドレス
```

### Passengerをインストールする

WEBrick経由だと若干反応が遅いのでPassengerをインストールする。「**SELinux**」は「**enforced**」の設定にしておく。またPassengerは[脆弱性の関係](https://blog.phusion.nl/2015/03/31/passenger-5-0-6/)で5.0.6以上にする。

```:bash
# Passengerをインストールするのに必要なパッケージをインストールする
yum install libcurl-devel /
httpd-devel /
apr-devel /
apr-util-devel

# Passengerをインストールする
gem install passenger --version 5.0.6
passenger-install-apache2-module
```

### Apacheの設定ファイルを編集する

```:bash
vi /etc/httpd/conf.d/rails.conf

# Railsの画像ファイル・CSSファイル等へのアクセスを許可する設定。
# Apache 2.4のデフォルトではサーバ上の全ファイルへのアクセスが禁止されている。
&lt;Directory "/var/www/html/demo"&gt;
  Require all granted
&lt;/Directory&gt;

# Passengerの基本設定。
# passenger-install-apache2-module --snippet で表示された設定を記述。
# 環境によって設定値が異なるため以下の3行はそのまま転記せず、必ず
# passenger-install-apache2-module --snippet で表示されたものを使用すること。
#
LoadModule passenger_module /usr/lib64/ruby/gems/2.3.0/gems/passenger-5.0.6/buildout/apache2/mod_passenger.so
   &lt;IfModule mod_passenger.c&gt;
     PassengerRoot /usr/lib64/ruby/gems/2.3.0/gems/passenger-5.0.6
     PassengerDefaultRuby /usr/bin/ruby
   &lt;/IfModule&gt;

# Passengerが追加するHTTPヘッダを削除するための設定（任意）。
#
Header always unset "X-Powered-By"
Header always unset "X-Runtime"

# 必要に応じてPassengerのチューニングのための設定を追加（任意）。
# 詳しくはPhusion Passenger users guide(https://www.phusionpassenger.com/library/config/apache/reference/)参照。
PassengerMaxPoolSize 20
PassengerMaxInstancesPerApp 4
PassengerPoolIdleTime 864000
PassengerHighPerformance on
PassengerStatThrottleRate 10
PassengerSpawnMethod smart
PassengerFriendlyErrorPages off

RackBaseURI /rails

# RailsをDevelopment modeで運用する
RailsEnv development
```

### SELinuxを設定する

```:bash
grep PassengerAgent /var/log/audit/audit.log | audit2allow -M passengeragent
semodule -i passengeragent.pp

grep httpd /var/log/audit/audit.log |audit2allow -M passenger
semodule -i passenger.pp

chcon -R -h -t httpd_sys_content_t `passenger-config --root`

# httpdモジュール(soファイル)とpassenger.soでターゲットポリシーを同一にする
ls -lZ /usr/lib64/httpd/modules/

-rwxr-xr-x. root root system_u:object_r:httpd_modules_t:s0 libphp5-zts.so
-rwxr-xr-x. root root system_u:object_r:httpd_modules_t:s0 libphp5.so
-rwxr-xr-x. root root system_u:object_r:httpd_modules_t:s0 mod_access_compat.so
-rwxr-xr-x. root root system_u:object_r:httpd_modules_t:s0 mod_actions.so
-rwxr-xr-x. root root system_u:object_r:httpd_modules_t:s0 mod_alias.so
-rwxr-xr-x. root root system_u:object_r:httpd_modules_t:s0 mod_allowmethods.so
-rwxr-xr-x. root root system_u:object_r:httpd_modules_t:s0 mod_asis.so
.
.
.
.
.
cd /usr/lib64/ruby/gems/2.3.0/gems/passenger-5.0.23/buildout/apache2/
chcon system_u:object_r:httpd_modules_t:s0 mod_passenger.so 

# Railsのインストールディレクトリの権限を設定する
chcon -R -h -t httpd_sys_content_t rails_dir

systemctl restart httpd.service
```

### Railsに秘密鍵公開鍵を設定する

モード別に「/rails\_dir/config/secrets.yml」に適宜「secret\_key_base」を設定する。

### 参考サイト

  * [CentOS 7 : Ruby on Rails 4 ： Server World](http://www.server-world.info/query?os=CentOS_7&#038;p=rails4)
  * [CentOS 7 : Ruby 2.2 インストール ： Server World](http://www.server-world.info/query?os=CentOS_7&#038;p=ruby22)
  * [CentOS7.1 64bit rpmbuildコマンドによるRPMの作成 | kakiro-web カキローウェブ](http://www.kakiro-web.com/linux/rpmbuild.html)
  * [Linux &#8211; SRPMを一撃でRPMビルドする。 (rpmコマンドでつい忘れがちな細かいオプションもおまけで記載) &#8211; Qiita](http://qiita.com/koitatu3/items/49635de6ec40a5f30222)
  * [How To Install Ruby on Rails with rbenv on CentOS 7 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-centos-7)
  * [Rails3 事始め: Passenger で development モードで動かす](http://rails3try.blogspot.jp/2011/12/passenger-development.html)
  * [SELinuxが有効な状態でRedmineを動かす &#8211; 開発メモ](http://seeku.hateblo.jp/entry/2013/05/31/093124)