---
title: CentOS7にffmpegをインストールする
author: tomonori
type: post
date: 2015-08-11T04:47:03+00:00
url: /?p=1018
categories:
  - CentOS7
  - ffmpeg

---
壮年団の動画をWindows7で編集しようとしたが、PCへの負担も大きく、HDDの消耗や故障も懸念される事からさくらのVPS上のCentOS7にて動画編集が出来ないかどうか画策してみる。

### 事前準備

```:bash
yum groupinstall "Development Tools" "Development Libraries"
```

### yasmをインストールする

x264のビルドに必要になる

```:bash
cd /usr/local/src
wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
tar zvxf yasm-1.3.0.tar.gz 
cd yasm-1.3.0
./configure
make
make install
```

### x264をインストールする

ffmpeg のビルドに必要になる。

```:bash
cd /usr/local/src
git clone git://git.videolan.org/x264
cd x264
./configure --enable-shared
make
make install
```

### fdk-aacをインストールする

fdk-aac を使うなら入れる必要がある。ソースを[fdk-aacのサイト][1]からダウンロード

```:bash
cd /usr/local/src
tar xzf fdk-aac-0.1.3.tar.gz
cd fdk-aac-0.1.3
./configure
make
make install
```

### ffmpegをインストールする

```:bash
cd /usr/local/src
git clone git://source.ffmpeg.org/ffmpeg.git ffmpeg
cd ffmpeg

# 時間がかなりかかる（30分程度？）
./configure --enable-gpl --enable-nonfree --enable-libfdk_aac --enable-libx264
make
make install

# ショートカット作成
ln -s /usr/bin/ffmpeg /usr/local/bin/ffmpeg
```

### 設定ファイルを変更する

```:bash
vi /etc/ld.so.conf

############################
include ld.so.conf.d/*.conf
/usr/local/lib ←追記
############################

# 設定ファイルをリロード
ldconfig
```

### 参考サイト

  * [CentOS7 ffmpegをソースからインストール &#8211; わすれないうちにメモしよう][2]
  * [なちょこlinuxer &raquo; 玄箱サーバー構築 &raquo; 玄箱HG Fedora11 FFmpeg インストール][3]
  * [ffmpegインストール。: Ma note][4]

 [1]: http://sourceforge.net/projects/opencore-amr/files/fdk-aac/
 [2]: http://d.hatena.ne.jp/kt_hiro/20150101/1420094609
 [3]: http://atbiz.dip.jp/kuro-box/kuro-box_ffmpeg.shtml
 [4]: http://m97087yh.seesaa.net/article/415078356.html