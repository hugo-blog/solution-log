---
title: CentOS7でmod_encodingを利用する
author: tomonori
type: post
date: 2015-12-01T14:37:59+00:00
url: /?p=1093
categories:
  - Apache
  - CentOS7

---
想定される環境
  
OS: CentOS7
  
Apache: 2.4系

### mod_encodingをインストールする

```:bash
# httpd-develインストール
yum -y install httpd-devel

# mod_encodingをダウンロード
wget http://webdav.todo.gr.jp/download/mod_encoding-20021209.tar.gz

# Apache2版mod_encodingダウンロード
wget http://webdav.todo.gr.jp/download/experimental/mod_encoding.c.apache2.20040616

# Apache2版mod_encodingに差し替え
tar zxvf mod_encoding-20021209.tar.gz
/bin/mv mod_encoding.c.apache2.20040616 mod_encoding-20021209/mod_encoding.c

# iconv-hookライブラリインストール
cd mod_encoding-20021209/lib
./configure && make && make install

# ※Namazuの検索文字列の文字化け対処
cd ..

# mod_encodingパッチダウンロード
wget http://www.aconus.com/~oyaji/faq/mod_encoding.c-apache2.2-20060520.patch　

# mod_encodingパッチ施工
patch -p0 &lt; mod_encoding.c-apache2.2-20060520.patch

# Makefile作成
# Apache2.4はapxsのパスが「/usr/bin/apxs」なので注意!
./configure --with-apxs=/usr/bin/apxs --with-iconv-hook

# Makefile編集
vi Makefile
##############################
LIBS =  -liconv_hook
↓
LIBS = -L/usr/local/lib -liconv_hook

install-exec-local:
        $(APXS) -i mod_encoding.so
        ↓
(Tab)   $(APXS) -i -a -n encoding mod_encoding.la
#####################################

# mod_encodingインストール
make && make install　←　

# mod_encoding展開先ディレクトリおよびダウンロードファイルを削除
cd　..
rm -rf mod_encoding-20021209
rm -f mod_encoding-20021209.tar.gz
```

### mod_encodingの設定をする

```:bash
# mod_encodingが動的に読み込まれているか確認
httpd -M | grep encoding_module

vi /etc/httpd/conf.d/mod_encoding.conf

&lt;IfModule mod_encoding.c&gt;
    EncodingEngine on
    SetServerEncoding UTF-8
    DefaultClientEncoding JA-AUTO-SJIS-MS SJIS
    AddClientEncoding "cadaver/" EUC-JP
&lt;/IfModule&gt;
```

### 参考サイト

  * [Webフォルダサーバー構築(WebDAV) &#8211; CentOSで自宅サーバー構築](http://centossrv.com/webdav.shtml) 
  * [CentOS7.1 64bitにPHP5.6.10をRPMからインストール | kakiro-web カキローウェブ](http://www.kakiro-web.com/linux/php-install.html)