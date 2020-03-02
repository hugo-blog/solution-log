---
title: Virtual BoxにインストールしたCentOS6.4でネットワークを設定する
author: tomonori
type: post
date: 2013-12-11T03:23:04+00:00
url: /?p=907
categories:
  - Virtual Box

---
Virtual Box上でCentOS環境を複数作成してテストスクリプトを試す必要が出てきた。その場合のネットワークを設定に関するまとめ。

### Ethernetインタフェースを設定する

```:bash
vi /etc/sysconfig/network-script/ifcfg-eth0

DEVICE="eth0"
HWADDR=XX:XX:XX:XX #Network Interface Card(NIC)のMACアドレス
TYPE=Ethernet
UUID=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXx
ONBOOT="yes"
MM_CONTROLLED="yes"
IPADDR=10.0.2.15
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8 #googleのDNSを指定
```

### ディフォルトゲートウェイを設定する

```:bash
service network restart

route add default gw 10.0.2.2

# 設定確認
route

Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
10.0.2.0        *               255.255.255.0   U     0      0        0 eth0
link-local      *               255.255.0.0     U     1002   0        0 eth0
default         10.0.2.2        0.0.0.0         UG    0      0        0 eth0
```

### 参考サイト

  * [Linux（CentOS5）のネットワーク設定メモ &#8211; プログラマ 福重 伸太朗 ～基本へ帰ろう～][1] 
      * [VirtualBox環境構築：CentOS &#8211; ネットワーク設定][2] 
          * 2[012-01-26 &#8211; 新人プログラマーのメモ帳][3] 
              * [エレクトリック・ボディ・ビート: Cent OS6でresolv.confが勝手に書き換わる。][4] 
                  * [VirtualBox を利用する際のネットワーク設定の話 &#8211; ゆどうふろぐ][5] 
                      * [ネットワーキング構成を理解して選択する : C-through the Mac][6] 
                          * [＠IT：デフォルトゲートウェイを変更するには][7] </ul>

 [1]: http://d.hatena.ne.jp/japanrock_pg/20080522/1211439429
 [2]: http://nextindex.s141.xrea.com/virtualbox/centos_network.html
 [3]: http://d.hatena.ne.jp/toshokan3210/20120126
 [4]: http://roserogue.blogspot.jp/2011/10/cent-os6resolvconf.html
 [5]: http://d.hatena.ne.jp/Yudoufu/20100117/1263677767
 [6]: http://c-through.blogto.jp/archives/14539119.html
 [7]: http://www.atmarkit.co.jp/flinux/rensai/linuxtips/401cngdefgw.html