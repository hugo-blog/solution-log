---
title: CentOS6でApache、PHP、MySQLの初期設定を行う
author: tomonori
type: post
date: 2013-03-16T16:27:06+00:00
url: /?p=758
categories:
  - Apache
  - CentOS6
  - MySQL
  - PHP

---
記事がバラバラになっているのでまとめておく。

### Apacheの自動機動設定を行う

想定される環境だと既にApacheはインストール済み。

```:bash
service httpd restart
chkconfig httpd on
```

### PHPをインストールする

こちらの[エントリー][1]を参考にする

### MySQL+phpMyAdminをインストールする

こちらの[エントリー][2]を参考にする

 [1]: ./?p=395
 [2]: ./?p=187