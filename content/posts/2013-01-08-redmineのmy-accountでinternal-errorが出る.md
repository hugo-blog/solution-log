---
title: RedmineのMy AccountでInternal Errorが出る
author: tomonori
type: post
date: 2013-01-08T10:34:00+00:00
url: /?p=568
categories:
  - Redmine

---
Redmineの初期設定で、adminでログインして、My Accountから日本語を設定しようとしたらInternal Errorが出た。

### RedmineのGemFileを使用する

結局のところ、/path/to/redmineのGemFileに記述してあるライブラリ通りの設定でないとRedmineが正しく機動しない事が判明。**RedmineのGemFile通りのライブラリを必ずインストールする事！**

### 参考リンク

http://assimane.blog.so-net.ne.jp/2011-01-03
  
http://www10.atwiki.jp/bambooflow/pages/273.html
  
http://blog.redmine.jp/articles/redmine-1\_2-installation\_centos/
  
http://blog.ruby.iijgio.com/2012/12/27/redmine220