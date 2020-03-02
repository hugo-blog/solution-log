---
title: CentOS6にGIMPをインストールする
author: tomonori
type: post
date: 2012-11-09T12:14:31+00:00
url: /?p=97
categories:
  - CentOS6
  - GIMP

---
そろそろ[GIMP][1]を覚えようと思いインストール。ちなみにGIMPとは「The GNU Image Manipulation Program」の略。

### GIMPをインストールする

```:bash
yum install gimp
```

gimp -vでバージョンが確認出来る。

```:bash
[root@localhost ~]# gimp -v
GNU Image Manipulation Program ver.2.6.9&lt;/samp&gt;&lt;/samp&gt;GEGL ver.0.1.2 を使用 (コンパイル済のバージョンは ver.0.1.2)
GLib ver.2.22.5 を使用 (コンパイル済のバージョンは ver.2.22.5)
GTK+ ver.2.18.9 を使用 (コンパイル済のバージョンは ver.2.18.9)
Pango ver.1.28.1 を使用 (コンパイル済のバージョンは ver.1.28.1)
Fontconfig ver.2.8.0 を使用 (コンパイル済のバージョンは ver.2.8.0)
```

出来れば最新の2.8系を使いたいのだが、ソールからインストールするときは、どうやら[システムをいろいろと弄る][2]ハメになるので(危ない。。。)、最新の2.8系を使うのは暫く様子見。

しかしPhotoshopと使い勝手が違うので若干操作しにくい。。。

 [1]: http://www.gimp.org/
 [2]: http://www.spinics.net/lists/gimp/msg26063.html