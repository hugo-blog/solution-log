---
title: Windows7にVirtualBoxをインストールしCentOS7をインストールする
author: tomonori
type: post
date: 2015-08-11T03:47:53+00:00
url: /?p=1003
categories:
  - CentOS7
  - Virtual Box
  - Windwos 7

---
ノートPCをlenovoからdynabookに替えたので新しく作業環境を整える必要が出てきた。CentOSも世の中の流れがだんだんと「6」から「7」になってきたのでローカル環境もCentOS7に統一する。

### Windows7にVirtualBoxをインストールする※注意

[VirtualBoxの公式サイトより](https://www.virtualbox.org/wiki/Downloads)最新のものをダウンロードしインストールする。**既にVirtualBoxがインストールされている場合は必ず最新バージョンのVirtualBoxを利用する**。

### CentOS7を取得する

ゲストOSをデスクトップ環境として使用する場合はCentOS7の公式サイトより[Everything ISO](https://www.centos.org/download/)をダウンロードする。ファイルサイズが7G以上あるのでダウンロードには若干時間がかかる。

### VirtualBoxにCentOS7をインストールする

『**ソフトウェアの選択(S)**』を選択し、左の ベース環境 から 『**GNOME Desktop**』 を選択
  
[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_1-300x220.jpg" alt="" width="300" height="220" class="aligncenter size-medium wp-image-1187" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_1-300x220.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_1-768x564.jpg 768w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_1-900x661.jpg 900w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_1.jpg 1013w" sizes="(max-width: 300px) 100vw, 300px" />][1]

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_2-300x196.jpg" alt="" width="300" height="196" class="aligncenter size-medium wp-image-1188" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_2-300x196.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_2-768x502.jpg 768w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_2-900x588.jpg 900w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_2.jpg 1013w" sizes="(max-width: 300px) 100vw, 300px" />][2]

『**ネットワークとホスト名(N)**』を選択し、ネットワーク接続を『**オン**』にする。

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_3-300x266.jpg" alt="" width="300" height="266" class="aligncenter size-medium wp-image-1189" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_3-300x266.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_3-768x680.jpg 768w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_3.jpg 850w" sizes="(max-width: 300px) 100vw, 300px" />][3]

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_4-300x44.jpg" alt="" width="300" height="44" class="aligncenter size-medium wp-image-1190" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_4-300x44.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_4-768x113.jpg 768w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_4-900x132.jpg 900w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_4.jpg 1017w" sizes="(max-width: 300px) 100vw, 300px" />][4]

### VirtualBox上でネットワークの設定を行う

Wi-fiを使わない場合はNATで良い。

『**VirtualBox**』→『**設定**』→『**ネットワーク**』の順に開き、『**アダプター 1**』の割り当ての項目を『**ブリッジアダプター**』に設定する。

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/150811_virtualbox_network_adapter_setting-300x169.jpg" alt="150811_virtualbox_network_adapter_setting" width="300" height="169" class="aligncenter size-medium wp-image-1007" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/150811_virtualbox_network_adapter_setting-300x169.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/150811_virtualbox_network_adapter_setting-1024x576.jpg 1024w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/150811_virtualbox_network_adapter_setting.jpg 1366w" sizes="(max-width: 300px) 100vw, 300px" />][5]

また『**名前**』の部分はWindows7がインターネット接続に利用するNICによって設定が変わるのでゲストOSの起動時に都度都度変更する必要がある。

  * **iPhone6をUSB接続でつないだ場合**→『**Apple Mobile Device Ethernet**』
  * **dynabookのwi-fiで接続する場合**→『**Realtek RTL8188CE Wireless LAN 802.1 1n PCI-E NIC**』

### VirtualBox Guest AdditionsをCentOS7にインストールする

初期の状態だとVirtualBoxにインストールしたCentOS7のモニター解像度が変更出来ない。そのためゲストOSのCentOS7に『**VirtualBox Guest Additions**』をインストールする。

ゲストOSであるCentOS7上で

```:bash
# あらかじめVirtualBox Guest Additionのインストールに
# 必要なライブラリやヘッダファイルをインストールする

yum install epel-release
yum install dkms
yum groupinstall "Development Tools"
yum install kernel-devel
yum update
```

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/150811_virtualbox_install_guest_addtions-300x169.jpg" alt="150811_virtualbox_install_guest_addtions" width="300" height="169" class="aligncenter size-medium wp-image-1011" />][6]

ゲストOSが立ち上がってる状態で

『**VirtualBox**』→『**デバイス**』→『**Guest AdditionsのCDイメージを挿入**』をクリック。あとは自動的に「**VirtualBox Guest Addtions**」がインストールされる。

Kernelとkernel-develのバージョンが同じでない場合は[インストールが失敗する][7]。yum updateを実行すると良い。

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_5-300x202.jpg" alt="" width="300" height="202" class="aligncenter size-medium wp-image-1191" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_5-300x202.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_5-768x516.jpg 768w, http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_5.jpg 783w" sizes="(max-width: 300px) 100vw, 300px" />][8]

### 共有フォルダの設定を行う

[こちら](http://qiita.com/HirofumiYashima/items/6044cfc64cfa3e84f97c)を参考にする

### 参考サイト

  * [CentOS &bull; View topic &#8211; CentOS 7 with VirtualBox](https://www.centos.org/forums/viewtopic.php?f=47&#038;t=47116)
  * [Guest Additionsのインストール | VirtualBox Mania](http://vboxmania.net/content/guest-additions%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
  * [VirtualBox (Windows) 上に CentOS 7 をインストールする &#8211; Qiita](http://qiita.com/100/items/80a899fbaeb1e82b3f67)
  * [【 Virtual Box 】共有フォルダを介して、ホストOS(Windows 7) と ゲストOS(Debian/ MathLibre)でファイルやりとりするための設定 &#8211; Qiita](http://qiita.com/HirofumiYashima/items/6044cfc64cfa3e84f97c)
  * [VirtualBox 上の CentOS 7.2 に Guest Additions をインストールする &#8211; Qiita](http://qiita.com/bezeklik/items/5600a22addd9fa5f04f5)

 [1]: http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_1.jpg
 [2]: http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_2.jpg
 [3]: http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_3.jpg
 [4]: http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_4.jpg
 [5]: http://13.112.229.114/wordpress/wp-content/uploads/2015/08/150811_virtualbox_network_adapter_setting.jpg
 [6]: http://13.112.229.114/wordpress/wp-content/uploads/2015/08/150811_virtualbox_install_guest_addtions.jpg
 [7]: http://qiita.com/bezeklik/items/5600a22addd9fa5f04f5
 [8]: http://13.112.229.114/wordpress/wp-content/uploads/2015/08/160510_install_centos7_on_vb_5.jpg