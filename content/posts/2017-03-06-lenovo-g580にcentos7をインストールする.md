---
title: Lenovo G580にCentOS7をインストールする
author: tomonori
type: post
date: 2017-03-05T16:20:48+00:00
url: /?p=1239
categories:
  - CentOS7

---
### Bootメディアを作成する

<del datetime="2017-03-06T02:11:41+00:00">SDカードを利用する。</del>CentOSのサイトより [Everything ISO](https://www.centos.org/download/) イメージをダウンロードしSDカードへコピーする。SDカードは**NTFS形式でフォーマット**しないと4GB以上のデータがSDカードにコピー出来ないので注意する。

**G580はDVDドライブからしかブート出来無い。**

```:bash
# mkfsをインストールする
yum install epel-release
yum install ntfsprogs

# SDカードをNTFS形式でフォーマットする
mkfs -t ntfs -F /dev/sdb
```

### Bootメディアを起動する

G580を立ち上げてすぐに「F12」を押す。「Boot Menu」からブートメディアを選択する。

### Minimal イメージからインストールする場合の手順

Bootメディアより一通りインストール手順を踏んだ後デスクトップ環境を構築する。

```:bash
# ネットワークへ接続する
nmtui

# もしくは
nmcli device # 有効なNICを調べる
nmcli connection up enp3s0

# 再起動時にNICが自動でネットワークへ接続するように設定を変更する
vi /etc/sysconfig/network-scripts/ifcfg-enp3s0 

ONBOOT="yes" # yesへ変更する


# システムをアップデートする
yum update

# デスクトップ環境をインストールする
yum groupinstall "GNOME Desktop"

# 常にグラフィカルモードにて起動するように設定を変更する
systemctl get-default
systemctl set-default graphical.target

# デスクトップを立ち上げる
startx
```

### マウスパッドの設定を行う

デスクトップから『**アプリケーション**』→『**システムツール**』→『**設定**』→『**マウスとタッチパッド**』を開き必要項目を設定する。

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2017/03/170306_centos7_mousepad_setting.jpg" alt="" width="742" height="349" class="aligncenter size-full wp-image-1244" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2017/03/170306_centos7_mousepad_setting.jpg 742w, http://13.112.229.114/wordpress/wp-content/uploads/2017/03/170306_centos7_mousepad_setting-300x141.jpg 300w" sizes="(max-width: 742px) 100vw, 742px" />][1]

### 参考サイト

#### ネットワークの設定方法について

  * [CentOS7 ネットワークの設定変更](http://www.unix-power.net/centos7/network.html)
  * [CentOS 7でのネットワーク設定 | 俺的備忘録 〜なんかいろいろ〜](https://orebibou.com/2014/07/centos-7%E3%81%A7%E3%81%AE%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E8%A8%AD%E5%AE%9A/)

#### SDカードのフォーマット方法について

  * [64GBのUSBメモリー &#8211; Webデザインを勉強する|元製版オペの奮闘記](http://d.hatena.ne.jp/practice4prepressman/20131003/p1)
  * [備忘録 for me CentOS(Linux)でntfsにフォーマットする](http://sugakun.blog.fc2.com/blog-entry-3.html)
  * [linux &#8211; How do I script mkfs asking "is entire device, not just one partition! Proceed anyway?" &#8211; Server Fault](http://serverfault.com/questions/413496/how-do-i-script-mkfs-asking-is-entire-device-not-just-one-partition-proceed-a)

 [1]: http://13.112.229.114/wordpress/wp-content/uploads/2017/03/170306_centos7_mousepad_setting.jpg