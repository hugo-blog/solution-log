---
title: Subversionコマンドメモ
author: tomonori
type: post
date: 2012-12-13T13:07:47+00:00
url: /?p=434
categories:
  - Subversion

---
http://den2sn.hatenablog.com/entry/20100326/1269589662

どうもうまく行かない。。。。

```:bash
svnadmin dump /home/smokingarage/var/svn/mvc | svndumpfilter include --drop-empty-revs --renumber-revs /home/smokingarage/var/svn/mvc/neriphp/trunk &gt; neri.dmp
svnadmin load /home/smokingarage/var/svn/neriphp &lt; /home/smokingarage/neri.dmp
```