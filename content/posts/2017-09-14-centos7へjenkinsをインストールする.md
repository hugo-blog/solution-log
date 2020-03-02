---
title: CentOS7へJenkinsをインストールする
author: tomonori
type: post
date: 2017-09-14T12:51:12+00:00
url: /?p=1317
categories:
  - CentOS7
  - Jenkins

---
### Jenkinsをインストールする

```:bash
# Javaをインストールする
yum install java

# yumリポジトリにjenkinsのリポジトリを追加する
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

# Jenkinsをインストールする
yum install jenkins

# Jenkinsを起動する
systemctl start jenkins

# Jenkinsが利用するポートを解放する
firewall-cmd --zone=public --permanent --add-port=8080/tcp
firewall-cmd --reload

# システムのロケールを変更する
localectl set-locale LANG=ja_JP.utf8
source /etc/locale.conf
```

### 参考サイト

  * [CentOS7へのJenkinsのインストール手順 &#8211; Qiita](http://qiita.com/yukimunet/items/28c89370fccc86077dd2)
  * [[CentOS]CentOS7でのロケール(locale)の確認及び変更 | Zero Configuration](http://zero-config.com/centos/changelocale-002.html)
  * [Jenkinsの文字コードを確認・設定する方法 &#8211; Qiita](http://qiita.com/LowSE01/items/3e39bf6f86e6884d69b1)