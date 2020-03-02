---
title: CentOS6.3でApache上でRedmineを実行する
author: tomonori
type: post
date: 2012-12-31T05:24:12+00:00
url: /?p=459
categories:
  - Redmine
tags:
  - apache
  - CentOS
  - ruby

---
[こちらのサイト][1]を参考にした。

### Passengerのインストール

[Phusion Passenger][2]をインストールする

```:bash
gem install passenger --no-rdoc --no-ri
```

### PassengerのApache用モジュールのインストールと設定

```:bash
service httpd start # Apacheを機動させておく
passenger-install-apache2-module
```

『**/etc/httpd/conf.d/passenger.conf**』を作成し「**passenger-install-apache2-module &#8211;snippet**」で表示される設定を書き込む。

```:bash
# Passengerの基本設定。
# passenger-install-apache2-module --snippet を実行して表示される設定を使用。
# 環境によって設定値が異なりますので以下の3行はそのまま転記しないでください。
#
LoadModule passenger_module /usr/local/lib/ruby/gems/1.9.1/gems/passenger-3.0.17/ext/apache2/mod_passenger.so
PassengerRoot /usr/local/lib/ruby/gems/1.9.1/gems/passenger-3.0.17
PassengerRuby /usr/local/bin/ruby
```

### Apacheの起動および自動起動の設定

```:bash
/etc/init.d/httpd start
chkconfig httpd on
```

### Apache上のPassengerでRedmineを実行するための設定

サブディレクトリでRedmineを実行する設定をする。シンボリックリンクを作成し、URLのディレクトリ名部分で使いたい名前を設定する。

```:bash
ln -s /var/lib/redmine/public /var/www/html/redmine
```

『**/etc/httpd/conf.d/passenger.conf**』に以下の設定を追加。

```:bash
RackBaseURI /redmine
```

また[Redmineインストールディレクトリの権限の設定][3]も行う事。

```:bash
chown -R apache:apache /var/lib/redmine
```

設定後、Apacheを再起動。

```:bash
/etc/init.d/httpd configtest
/etc/init.d/httpd graceful
```

### 参考サイト

  * [Redmine 2.1をCentOS 6.3にインストールする手順 | Redmine.JP Blog][1] </ul>

 [1]: http://blog.redmine.jp/articles/redmine-2_1-installation_centos/
 [2]: http://www.modrails.com/
 [3]: ./?p=605