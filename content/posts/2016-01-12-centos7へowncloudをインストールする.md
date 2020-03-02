---
title: CentOS7へownCloudをインストールする
author: tomonori
type: post
date: 2016-01-12T06:48:12+00:00
url: /?p=1122
categories:
  - CentOS7
  - owncloud

---
### リポジトリを追加する

RPMForge、EPEL、Remiをリポジトリへ追加する

```:bash
# RPMForge
rpm -ivh http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm
rpm --import http://dag.wieers.com/rpm/packages/RPM-GPG-KEY.dag.txt
 
# EPEL
rpm -ivh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
rpm --import http://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7
 
# remi
rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
rpm --import http://rpms.famillecollet.com/RPM-GPG-KEY-remi
```

### PHPをインストールする

```:bash
# gd-lastをインストール
yum -y --enablerepo=remi install gd-last

# MariaDBをインストールする
yum -y install mariadb*

yum -y --enablerepo=remi-php56 install php \
php-cli \
php-common \
php-gd \
php-ldap \
php-mbstring \
php-pdo \
php-process \
php-xml \
php-mysqlnd

# memcached(分散型メモリキャッシュシステム)をPHPで使えるようにする
yum install memcached-devel
yum -y install php-pecl-memcache
```

### PHPを設定する

```:bash
# php.confの設定
vi /etc/httpd/conf.d/php.conf

AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .phps

# php.iniの設定
vi /etc/php.ini

date.timezone = Asia/Tokyo
```

### ownCloudをインストールする

ユーザーディレクトリで動かせるようにソースからインストールする

```:bash
cd /home/user/
curl -O https://download.owncloud.org/community/owncloud-8.2.2.zip
unzip owncloud-8.2.2.zip
```

### データベースを作成する

### 参考サイト

  * [CentOS 7.1 (1503) LAMPサーバインストールメモ【CentOS7.1.1503＋Apache＋MySQL＋PHP】 | あぱーブログ](https://blog.apar.jp/linux/2262/)
  * [owncloudを8.1？にしたらいろいろ出た | 世界を疑え](http://astail.net/?p=906)