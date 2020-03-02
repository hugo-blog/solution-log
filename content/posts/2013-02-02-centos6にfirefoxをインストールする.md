---
title: CentOS6にFireFoxをインストールする
author: tomonori
type: post
date: 2013-02-02T11:16:38+00:00
url: /?p=653
categories:
  - CentOS6
  - FireFox

---
VNC環境を整え、VNCサーバ上でもFireFoxが使えると便利だと思い試してみる。

### FireFoxをインストールする

```:bash
yum install firefox
```

### FireFoxをアップデートする

remiリポジトリがインストールされているのを確認したら

```:bash
yum --enablerepo=remi update firefox
```

### FLASHプラグインをインストールする

```:bash
## Adobe Repository 32-bit x86 ##
rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-i386-1.0-1.noarch.rpm
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux
 
## Adobe Repository 64-bit x86_64 ##
rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-x86_64-1.0-1.noarch.rpm
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux

# Update Repositories
yum check-update

yum install flash-plugin nspluginwrapper alsa-plugins-pulseaudio libcurl
```

### 参考サイト

  * [CentOS 6.2に最新版のFirefoxとFlash Playerをインストール &#8211; なんじゃくにっき][1] 
      * [Adobe Flash Player 11.2 on Fedora 18/17, CentOS/RHEL 6.3/5.8][2] </ul>

 [1]: http://d.hatena.ne.jp/nanjakkun/20111229/1325169540
 [2]: http://www.if-not-true-then-false.com/2010/install-adobe-flash-player-10-on-fedora-centos-red-hat-rhel/http://