---
title: ImageMagicで画像ファイルをPDF化する
author: tomonori
type: post
date: 2015-04-27T09:37:08+00:00
url: /?p=977
categories:
  - ImageMagic

---
画像ファイルをPDF化する機会があったのでImageMagicを使い、レンタルサーバで処理させてみた。

### ImageMagciをインストールする

```:bash
yum install ImageMagick ImageMagick-devel
```

### 画像データをPDF化する

```:bash
# ファイルを作成日の古い順にPDF化する
convert `ls -tr` target.pdf
```

### 参考サイト

  * [複数のjpegを一つのpdfへ変換した時のメモ &#8211; プログラムとかののblog][1]

 [1]: http://d.hatena.ne.jp/pogin/20120727/1343408520