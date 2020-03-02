---
title: CentOS6.5にVMWare Playerをインストールする
author: tomonori
type: post
date: 2014-05-01T15:21:43+00:00
url: /?p=938
categories:
  - VMware Player

---
今までインストール手順をまとめてなかったのでまとめておく

### VMWare Playerをダウンロードする

yumコマンドで簡単にインストール出きるのかと思ったがそうでもないらしい。インストールパッケージを[VMWare Playerのサイト][1]からダウンロードする必要がある。

### VMWare Playerをインストールする

ダウンロードしたファイルをrootで実行する。

```:bash
su root
bash VMware-Player-6.0.2-1744117.i386.bundle 
```

### 参考サイト

  * [Ubuntu 12.04 に VMware Player Linux 版 バージョン 5.0.0 のインストール][2] </ul>

 [1]: https://my.vmware.com/jp/web/vmware/downloads
 [2]: http://www.kkaneko.com/rinkou/vmwareplayer/vmwareplayerubuntu.html