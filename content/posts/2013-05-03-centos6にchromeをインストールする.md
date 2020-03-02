---
title: CentOS6にChromeをインストールする
author: tomonori
type: post
date: 2013-05-02T15:37:24+00:00
url: /?p=807
categories:
  - CentOS6
  - Chrome

---
どうもCentOSとFireFoxが相性が悪いのでChromeに乗り換えてみる。

### Google YUM リポジトリを有効にする

```:bash
vi /etc/yum.repos.d/google-chorme.repo

# 64 bit 版の場合
[google-chrome]
name=google-chrome - 64-bit
baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

# 32 bit 版の場合
[google-chrome]
name=google-chrome - 32-bit
baseurl=http://dl.google.com/linux/chrome/rpm/stable/i386
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
```

### Chromeをインストールする

インストール可能なパッケージを調べる

```:bash
yum list "*chrome*"
Loaded plugins: fastestmirror, refresh-packagekit
Loading mirror speeds from cached hostfile
 * base: rsync.atworks.co.jp
 * epel: ftp.jaist.ac.jp
 * extras: rsync.atworks.co.jp
 * updates: rsync.atworks.co.jp
google-chrome                                 |  951 B     00:00     
google-chrome/primary                         | 1.4 kB     00:00     
google-chrome                                 3/3
Installed Packages
xorg-x11-drv-openchrome.x86_64                0.2.904-4.el6       @base        
Available Packages
google-chrome-beta.x86_64                     18.0.1025.39-122785 google-chrome
google-chrome-stable.x86_64                   17.0.963.56-121963  google-chrome
google-chrome-unstable.x86_64                 19.0.1041.0-121843  google-chrome
xorg-x11-drv-openchrome.i686                  0.2.904-4.el6       base         
xorg-x11-drv-openchrome-devel.i686            0.2.904-4.el6       base         
xorg-x11-drv-openchrome-devel.x86_64          0.2.904-4.el6       base
```

Chromeをインストールする

```:bash
# 現行版をインストールする
sudo yum install google-chrome-stable

# ベータ版をインストールする。
sudo yum install google-chrome-beta

# unstable版をインストールする。
sudo yum install google-chrome-unstable
```

### 参考サイト

  * [CentOS 6.2 に Chrome をインストールする][1] </ul>

 [1]: http://kaworu.jpn.org/kaworu/2012-02-10-1.php