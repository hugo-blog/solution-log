---
title: Windows7にVMware Player + CentOS6.3をインストールする
author: tomonori
type: post
date: 2013-01-03T09:01:31+00:00
url: /?p=535
categories:
  - VMware Player
  - Windwos 7

---
WindowsでのSSHやgitの設定が非常に手間なのでノートPCにCentOSをそのまま突っ込もうと思いVMware Playerを試す。

### CentOS6.3のディスクイメージを入手する

VMware Playerの仮想OSとして使用するCentOS6.3のディスクイメージを[ダウンロードサイト][1]から入手。ダウンロード先によって接続スピートがかなり異なるので速いところを選ぶ。今の所外付けのHDD内にダウンロード済みのCentOS6.3のディスクイメージがあるのでそちらを使う。

### VMware Playerをインストールする

[VMware Playerのサイト][2]からWindows用のインストールファイルを[ダウンロードして][3]通常通りインストールする。

### 仮想OSの設定ファイルを作成する

VMware Playerはのみの対応なので、CentOSが日本語にならない。なので[EasyVMX!のサイト][4]で仮想OSの設定ファイルを作成する。

  1. easyvmx 2.0をクリック 
      * 仮想OS名、割り当てHDD容量などを記入して「Create Virtual Machine」をクリック 
          * (仮想マシン名).zip というダウンロードリンクが表示されるので、クリックしてダウンロード。 </ol> 
            ### CentOS6.3をインストールする
            
            前の手順で作成したZIPファイルを仮想OSをインストールするフォルダに展開する。テキストエディタなどで(仮想OS名).vmxファイルを開いて編集。
            
            ```:bash
ide1:0.deviceType = "cdrom-image"
ide1:0.fileName = "(isoイメージのあるパス)\centosdvd.iso"
```
            
            その後(仮想OS名).vmxをダブルクリック。後はOSの指示通りにインストール作業を進めて行く。
            
            ### 参考サイト
            
              * [www.centos.org &#8211; The Community ENTerprise Operating System][5] 
                  * [VMWare Player メモ &#8211; MLEXP Wiki][6] 
                      * [VMware の仮想化によるインフラストラクチャの最適化][7] </ul>

 [1]: http://isoredirect.centos.org/centos/6/isos/i386/
 [2]: http://www.vmware.com/
 [3]: http://www.vmware.com/support/product-support/player.html
 [4]: http://www.easyvmx.com/
 [5]: http://www.centos.org/
 [6]: http://www.mlexp.com/wiki/index.php?VMWare%20Player%20%A5%E1%A5%E2
 [7]: http://www.vmware.com/jp