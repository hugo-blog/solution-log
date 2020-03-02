---
title: HP Compaq 8100 Elite Small に nVIDIA GeForce GT520 1GB LowProfile を装着し CentOS7をインストールする
author: tomonori
type: post
date: 2017-07-03T15:09:29+00:00
url: /?p=1309
categories:
  - CentOS7
  - nVIDIA

---
### CentOS7を設定する

```:bash
# フォントを大きくする
setfont sun12x22

# インターネットへ接続する
nmtui

# GNOME デスクトップをインストールする
yum groupinstall "GNOME Desktop"

# nVIDIAグラフィックカードドライバーのインストールrpmをインストールする

rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
echo $(nvidia-detect)
yum install kmod-nvidia.x86_64


# ターゲットユニットを設定する
systemctl get-default
systemctl set-default graphical.target

 # 再起動する
reboot
```

### 参考サイト

  * [Please Help with NVIDIA Driver installation on Centos 7 &#8211; CentOS](https://www.centos.org/forums/viewtopic.php?t=61162)