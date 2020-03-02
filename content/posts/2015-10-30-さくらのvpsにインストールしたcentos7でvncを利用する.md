---
title: さくらのVPSにインストールしたCentOS7でVNCを利用する
author: tomonori
type: post
date: 2015-10-29T17:09:54+00:00
url: /?p=1038
categories:
  - CentOS7
  - VNC

---
### 1. 必要なパッケージのインストール

<h4 style="padding-left: 60px;">
  サーバ
</h4>

```:bash
yum install tigervnc-server
yum groupinstall "GNOME Desktop"
```

<h4 style="padding-left: 60px;">
  クライアント
</h4>

```:bash
yum install vnc
```

### VNCサーバの設定

```:bash
cp /lib/systemd/system/vncserver@.service \
/etc/systemd/system/vncserver@:1.service

vi /etc/systemd/system/vncserver@:1.service

#&lt;USER&gt;をVNC USERに書き換える
ExecStart=/sbin/runuser -l vncuser -c "/usr/bin/vncserver %i"
PIDFile=/home/vncuser/.vnc/%H%i.pid

# systemdに設定ファイルを再度読込みをさせる。
systemctl daemon-reload
```

### VNCユーザの設定

ログインユーザーをVNCユーザーに切り替えてパスワードを変更する

```:bash
su vncuser
vncpasswd &lt;enter&gt;
Password: ****** &lt;enter&gt;
Verify: ***** &lt;enter&gt;
```

### ファイアウォールの設定

```:bash
firewall-cmd --permanent --add-service vnc-server
systemctl restart firewalld.service
```

### VNCサービスの起動

```:bash
systemctl start vncserver@:1.service

# VNCサービスの自動起動設定を行う
systemctl enable vncserver@:1.service
```

### クライアントからVNCを使う

F8でフルスクリーンモードの切り替え設定が可能になる

<h4 style="padding-left: 60px;">
  デスクトップから起動する
</h4>

<p style="padding-left: 60px;">
  <b>アプリケーション</b>→<b>インターネット</b>→<b>TigerVNC Viewver</b>
</p>

<h4 style="padding-left: 60px;">
  端末から起動する
</h4>

```:bash
vncviewer yourhost:1
```

### 参考サイト

[[CentOS]CentOS7最小限のインストールからのGnomeデスクトップ環境構築 | Zero Configuration](http://zero-config.com/centos/gnome-0001.html)
  
[CentOS 7にVNCサーバをインストールする | 俺的備忘録 〜なんかいろいろ〜](http://orebibou.com/2014/12/centos-7%E3%81%ABvnc%E3%82%B5%E3%83%BC%E3%83%90%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B/)
  
[CentOS 7でVNCサーバを構築してみた &#8211; Qiita](http://qiita.com/tanuki-project/items/2496339d204b9646f36c)