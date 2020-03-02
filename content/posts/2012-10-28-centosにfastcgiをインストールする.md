---
title: CentOSにFastCGIをインストールする
author: tomonori
type: post
date: 2012-10-28T10:54:31+00:00
url: /?p=43
categories:
  - CentOS6
  - FastCGI

---
どうやらRubyをCentOSで動かすにはFastCGIと言って、動作をキャッシュするような仕組みをインストール刷る必要があるらしい。

### FastCGIをインストールする

```:bash
cd /usr/local/src/
wget http://www.fastcgi.com/dist/mod_fastcgi-2.4.6.tar.gz
tar xzfz mod_fastcgi-2.4.6.tar.gz
cd mod_fastcgi-2.4.6
cp Makefile.AP2 Makefile
make top_dir=/usr/lib/httpd
make top_dir=/usr/lib/httpd install
```

**makeコマンドで認識刷るファイルはMakefileと言う名前のみなので、mod_fastcig-2.4.6フォルダ内のMakefile.AP2をMakefileにコピーして変更する**。

### 参考サイト

  * [FastCGIのインストールと設定][1]

 [1]: http://www.movabletype.jp/documentation/developer/server/fastcgi.html "FastCGIのインストールと設定"