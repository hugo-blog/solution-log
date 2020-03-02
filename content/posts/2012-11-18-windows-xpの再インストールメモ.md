---
title: Windows XPの再インストールメモ
author: tomonori
type: post
date: 2012-11-18T12:47:03+00:00
url: /?p=174
categories:
  - Windows XP
  - メモ

---
デスクトップにWindows XP環境を構築するときのメモ

### データのバックアップ

Subversionやgit等も使い、忘れずに行う。

### LANドライバをインストール

P5GC-MX/1333 CD-ROMから「Atheros L2 Ethernet Driver」をインストールする。これでインターネットに接続できるようになる。

### サービスパックを適用する

  1. マクロソフトのサイトからWindows XP SP2をダウンロード、インストール
  2. SP3をダウンロード、インストール
  
    SP3はSP2が適応されてないとインストール出来ないので注意</ol> 

### オーディオドライバをインストール

P5GC-MX/1333 CD-ROMから「Realteck Audio Driver」をインストールする

### グラフィックカードドライバをインストール

Autorun DriverのラベルのCDからインストールする

### USBドライバをインストールする

「**D:\Drivers\Chipset\Inf**」のフォルダ内のsetup.exeを実行する。それ以外は宣伝用のソフトなのでいじらない。

後は適宜Lhaz等をインストールする。