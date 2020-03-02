---
title: CentOS7でUSBHDD（NTFS）をマウントする
author: tomonori
type: post
date: 2016-08-22T09:43:47+00:00
url: /?p=1197
categories:
  - 未分類

---
### 想定環境

  * **さくらのVPS 2GB**
  * **CentOS7**

dmesg
  
fdisk -l

yum install epel-release
  
yum -y &#8211;enablerepo=epel install dkms fuse-ntfs-3g

mkidr /mnt/d
  
mount /dev/sdc1 /mnt/d

### 参考サイト

[CentOS7でUSBディスク（NTFS）をマウントする &#8211; Open Source Software Forum](http://www.totsusangyo.com/netcommons/?page_id=57)
  
[Linux CentOS 外付けHDDのフォーマットとマウント](http://mrs.suzu841.com/mini_memo/numero_13.html)
  
[CentOS 5.6 でNTFSのUSB HDDをマウントする &#8211; 常水商会::よしなしごと::旧本店](http://d.hatena.ne.jp/kiyotune/20110509/1304918556)