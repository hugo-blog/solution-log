---
title: CentOS7へNVIDIAグラフィックスドライバをインストールする
author: tomonori
type: post
date: 2016-01-02T11:47:00+00:00
url: /?p=1117
categories:
  - CentOS7

---
CentOS7をアップデートするとグラフィックスドライバをカーネルに対して再構築しないといけないので手順をまとめておく

### NVIDIAグラフィックスドライバーを入手する

[NVIDIAのサイト](http://www.nvidia.co.jp/Download/index.aspx?lang=jp)よりダウンロードしておく

### ドライバをインストール、カーネルを再構築する

```:bash
yum update
reboot 

# 最新バージョンのカーネルを選択し起動させる
# もしGNOMEが立ち上がらない場合はCtl + Alt + F2でログインする

systemctl disable gdm
reboot

# グラフィックスドライバをインストールする
bash NVIDIA-Linux-x86_64-352.30.run

# ユーザーアカウントのダウンロードディレクトリなどにファイルがある場合
# CentOSのターミナルから日本語が読み込めないし打ち込めない場合
bash `find /home/tomonori -name "NVIDIA*"`

# シンボリックリンクを作成する前にサービスを起動する
systemctl enable gdm.service

# エラーが出たらオプションを-「f」に変更
ln -s "/usr/lib/systemd/system/gdm.service" "/etc/systemd/system/display-manager.service"

reboot
```

### 参考サイト

  * [NVIDIA GeForce Driver Installation on CentOS 7 Linux 64-bit](http://linuxconfig.org/nvidia-geforce-driver-installation-on-centos-7-linux-64-bit)