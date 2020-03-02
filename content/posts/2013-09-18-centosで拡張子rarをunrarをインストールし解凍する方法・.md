---
title: CentOSで拡張子RARをUnRARをインストールし解凍する方法・コマンド
author: tomonori
type: post
date: 2013-09-18T13:51:59+00:00
url: /?p=867
categories:
  - Linuxコマンド

---
CentOSで拡張子RARをUnRARをインストールし解凍する方法・コマンド

CentOSで拡張子RARをUnRARをインストールし解凍する方法・コマンド

■OS

CentOS 6.0

■拡張子

.rar

■インストール

UnRAR

■作業

CentOSで拡張子RARをUnRARをインストールし解凍

■CentOSで拡張子RARをUnRARをインストールし解凍する方法・コマンド

下記コマンドを実行し「rpmforge-release-0.3.6-1.el5.rf.i386.rpm」をダウンロード

wget http://dag.wieers.com/packages/rpmforge-release/rpmforge-release-0.3.6-1.el5.rf.i386.rpm
  
「rpmforge-release-0.3.6-1.el5.rf.i386.rpm」をインストール

rpm -ivh rpmforge-release-0.3.6-1.el5.rf.i386.rpm
  
「/etc/yum.repos.d/rpmforge.repo」を開く

vi /etc/yum.repos.d/rpmforge.repo
  
下記の内容に変更

enabled = 0
  
「UnRAR」をインストール

yum &#8211;enablerepo=rpmforge install unrar
  
拡張子「.rar」を解凍

unrar e test.rar

http://www.miuxmiu.com/archives/2011/08/02/centos\_extension\_rar\_unrar\_install\_unzip\_command.html