---
title: CentOS7でWindows Zipアーカイブを解凍する
author: tomonori
type: post
date: 2015-12-01T16:15:05+00:00
url: /?p=1096
categories:
  - CentOS7
  - Perl
  - unzip

---
Windowsで作成したzipアーカイブは通常のCentOS7だと解凍出来ないのでPerlを使う

### Archive::Zipをインストールする

CPAN経由だとパスが通らないのでyumを使う

```:bash
yum install perl-Archive-Zip
```

### zipアーカイブを解凍する

アーカイブを解凍するためのPerlスクリプトを作成する

```:bash
vi unzip-sjis.pl

##################################
#!/usr/bin/perl
use strict;
use warnings;
use Archive::Zip;
use Encode;

if (@ARGV != 1) {
  die "usage: $0 ZIPFILE\n";
}
my $zipfile = shift;
if (! -e $zipfile) {
  die "$zipfile don"t exist\n";
} 
my $zip=Archive::Zip-&gt;new($zipfile);
my @items = $zip-&gt;memberNames();
my $utf8name;
foreach (@items) {
  my $utf8name = $_;
  Encode::from_to($utf8name, "cp932", "utf-8");
  print "extract : $utf8name\n";
  $zip-&gt;extractMember($_, $utf8name);
}
#####################

# ファイルを解凍する
./unzip-sjis.pl windows-zip-archive.zip
```

### 参考サイト

  * [CentOS/CentOSでWindowsのシフトJISファイル名を含むzipファイルを解凍する方法 &#8211; Linuxと過ごす](http://linux.just4fun.biz/CentOS/CentOS%E3%81%A7Windows%E3%81%AE%E3%82%B7%E3%83%95%E3%83%88JIS%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%90%8D%E3%82%92%E5%90%AB%E3%82%80zip%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E8%A7%A3%E5%87%8D%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95.html)
  * [Perl関係のモジュールをyumインストール | 実験酒場](http://www.ohoclick.com/archives/44)