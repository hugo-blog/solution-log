---
title: CentOS6にJenkinsをインストールする
author: tomonori
type: post
date: 2012-11-30T15:59:15+00:00
url: /?p=201
categories:
  - CentOS6
  - Jenkins
tags:
  - apache

---
### Javaをインストールする

Javaがインストールされていない場合はインストールする。

```:bash
rpm -qa java-1.7.0-openjdk
yum -y install java-1.7.0-openjdk
```

### Jenkinsをインストールする

基本的には公式ページを参考にする。
  
<http://pkg.jenkins-ci.org/redhat/>

```:bash
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
yum install jenkins
```

### Jenkinsを起動する

Jenkinsはポート8080がディフォルト。

```:bash
httpd -k start
/sbin/service jenkins start
```

ブラウザから<http://localhost:8080/>へアクセス。

### JenkinsとApacheを連携させる

まずプロキシの設定をmod\_proxy経由で行う。mod\_proxyの設定はhttpd.confに書き込む事も出きるが、読みにくくなるのでmod_proxy.confに書き込む。

```:bash
cd /etc/httpd/conf.d
vi mod_proxy.conf
# 書き込み内容
ProxyPass /jenkins http://localhost:8080/jenkins
ProxyPassReverse /jenkins http://localhost:8080/jenkins
ProxyRequests Off
```

Jenkinsの設定ファイルを書き換える

```:bash
vi /etc/sysconfig/jenkins
JENKINS_ARGS="--prefix=/jenkins" #最終行を書き換え
```

ApacheとJenkinsを立ち上げれば、http://localhost/jenkinsよりJenkinsに入れる

```:bash
httpd -k stop
httpd -k start
/sbin/service jenkins start
```

### 参考サイト

  * [RedHat Linux RPM packages for Jenkins][1]
  * [Apache のリバースプロキシの設定方法 | WebOS Goodies][2]
  * [jenkinsとapacheの連携 &#8211; Dev3TechHack][3]

 [1]: http://pkg.jenkins-ci.org/redhat/
 [2]: http://webos-goodies.jp/archives/51261261.html
 [3]: http://d.hatena.ne.jp/hiranasu/20110507/1304781709