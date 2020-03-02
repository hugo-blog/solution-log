---
title: さくらのVPSにVirtualBox + vagrantをインストールする
author: tomonori
type: post
date: 2016-03-06T11:32:06+00:00
url: /?p=1168
categories:
  - CentOS7
  - vagrant
  - Virtual Box
  - さくらのVPS

---
Docker、Vagrant等仮想化技術が非常に大切だと感じるこの頃｡｡｡

### 想定環境

  * **さくらのVPS 2GB**
  * **CentOS7**

### VirtualBoxをインストールする

[こちらのサイト](http://qiita.com/Itomaki/items/9a6a314a853cdcd00f80)を参考にした。

```:bash
yum istall dkms

cd /etc/yum.repos.d/
sudo wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo

yum update

yum list | grep VirtualBox

yum install VirtualBox-xxxx
```

### Vagrantをインストールする

[公式サイト](https://www.vagrantup.com/downloads.html)よりrmpをダウンロードしインストーるする

```:bash
rpm -Uvh https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.rpm
```