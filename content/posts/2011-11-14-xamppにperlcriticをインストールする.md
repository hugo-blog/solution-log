---
title: XAMPPにPerl::Criticをインストールする
author: tomonori
type: post
date: 2011-11-14T11:43:31+00:00
url: /?p=229
categories:
  - Perl
  - XAMPP

---
前投稿からの続き
  
【ＸＡＭＰＰにPerltidyをインストールする】
  
<http://ameblo.jp/smackmybitchup/entry-11078527558.html>

環境
  
Windows XP ver3
  
XAMPP 1.7.3

Perltidyをインストールできたので勢いに任せてPerl::Criticもインストール
  
<http://perlcritic.com/>

まず、普通に
  
C:\xampp\perl\binから
  
【cpan Perl::Critic】
  
エラー。どうやらPPIと言うライブラリが必要らしい。*1
  
そこで
  
【cpan force PPI】
  
で、結局Perl::Criticの通常インストールもテストエラーに終わるので

【cpan force Perl::Critic】

*1

【 たまには呪文をとなえてみるか：仕事版 / おじいさんなので、Perl::Critic入れてみた】

<http://cast-a-spell.at.webry.info/200806/article_1.html>