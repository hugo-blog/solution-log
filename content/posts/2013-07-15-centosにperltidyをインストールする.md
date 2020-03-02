---
title: EclipseでPerl::Tidyを使用する
author: tomonori
type: post
date: 2013-07-15T13:47:40+00:00
url: /?p=832
categories:
  - Eclipse
  - Perl
tags:
  - cpan
  - Perltidy
  - YAML

---
Eclipse + EPICでPerlで開発を行う場合、コードフォーマッターらしきものがあった方が便利なのでPerl::Tidyをインストールしてみる。

### 開発者向けパッケージをインストールする

これをインストールしないとPerlでモジュールがインストール出来ない

```:bash
yum -y groupinstall "Development tools"

# インストールされるものを確認する
yum groupinfo "Development tools"
```

### libyaml-develをインストールする

最初にlibyaml-develをインストールしないと、cpan YAMLがインストール出来ない

```:bash
yum install libyaml-devel
yum -y install *YAML*
```

### Perl::Tidyをインストールする

想定される環境だとPerlはインストール済み。CPANをインストールし、cpan YAMLをインストールしてからPerl::Tidyをインストールする。

```:bash
yum install perl-CPAN

perl -MCPAN -e shell

# モジュールをアップデートしたりするときにyesコマンドを省略する
cpan &gt; o conf prerequisites_policy follow
cpan &gt; o conf commit

# かならずCPANを最新のものにしておく
cpan&gt; upgrade

# CPAN YAMLをインストールする
cpan&gt; install YAML
cpan&gt; install Perl::Tidy
```

Perl::Tidyのインストールが完了すれば、Eclipseの編集中のPerlファイル上で「**Ctrl+Shift+F**」でコードをフォーマットできるようになる。

### 参考サイト

  * [CPANのインストールからモジュールのインストールするまでの簡単な流れ &#8211; ヘッポコからフルスタックを目指す記録][1] 
      * [perltidyでソースコード整形 &#8211; 半径5メートル][2] 
          * [Aptana(Eclipse)+EPIC+PerltidyでPerl開発環境を整えて自動コードフォーマットで楽をする | 三度の飯とエレクトロン][3] 
              * [[O] PerlのCPANモジュールをインストールする時にyesを自動選択するには][4] 
                  * [[CPAN] Can't test without successful make を解消するの巻 &#8211; TrippyBoyの愉快な日々][5] </ul>

 [1]: http://fakeyakuza.hatenablog.com/entry/2012/12/14/CPAN%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%8B%E3%82%89%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%AB%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB
 [2]: http://d.hatena.ne.jp/srkzhr/20090408/1239206063
 [3]: http://blog.katty.in/24
 [4]: http://diary.overlasting.net/2010-01-14-3.html
 [5]: http://blog.trippyboy.com/2013/cpan/cpan-cant-test-without-successful-make-%E3%82%92%E8%A7%A3%E6%B6%88%E3%81%99%E3%82%8B%E3%81%AE%E5%B7%BB/