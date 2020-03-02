---
title: CentOS6でSubversionからgitに移行する
author: tomonori
type: post
date: 2012-12-02T11:59:34+00:00
url: /?p=290
categories:
  - CentOS6
  - Git
  - Subversion

---
一人プロジェクトの場合、Subversionみたいなガチガチのバージョン管理を使う必要も無いなと思う所もあり、gitに移行することにした。参考にしたのは[入門git][1]

### git-svnが利用できるか確認する

まずgitとSubversionを連携させるためのツールがインストールされているか確認する

```:bash
git svn --version
```

**Can&#8217;t locate SVN/Core.pm in @INC&#8230;**とエラーが出る。どうやらSubversion用のPerlのバインディングがインストールされていないらしい。とりあえずはyumで対応する。

```:bash
yum install subversion-perl
```

簡単に入る。何かしらgitプロジェクトを作成し、gitの作業ツリー内で再びバージョンを確かめると

```:bash
```

移行の準備は整った。

### Subversionリポジトリをインポートする

さくらのレンタルサーバにあるリポジトリからローカルにリビジョン番号を指定してインポートしてみる。

```:bash
cd /root/ダウンロード
git svn clone -r {start_rev}:{end_rev} svn+ssh://{username}@{domain}.sakura.ne.jp/home/{username}/var/svn/{repo}
```

### インポートされたリポジトリの確認

```:bash
cd git-repository
git log
```

操作方法が分からないときはhを入力するとHELPが表示される

```:bash
SUMMARY OF LESS COMMANDS

      Commands marked with * may be preceded by a number, N.
      Notes in parentheses indicate the behavior if N is given.

  h  H                 Display this help.
  q  :q  Q  :Q  ZZ     Exit.
 ---------------------------------------------------------------------------

                           MOVING

  e  ^E  j  ^N  CR  *  Forward  one line   (or N lines).
  y  ^Y  k  ^K  ^P  *  Backward one line   (or N lines).
  f  ^F  ^V  SPACE  *  Forward  one window (or N lines).
  b  ^B  ESC-v      *  Backward one window (or N lines).
  z                 *  Forward  one window (and set window to N).
  w                 *  Backward one window (and set window to N).
  ESC-SPACE         *  Forward  one window, but don"t stop at end-of-file.
  d  ^D             *  Forward  one half-window (and set half-window to N).
  u  ^U             *  Backward one half-window (and set half-window to N).
  ESC-)  RightArrow *  Left  one half screen width (or N positions).
  ESC-(  LeftArrow  *  Right one half screen width (or N positions).
HELP -- Press RETURN for more, or q when done
```

### リポジトリの分割

[git-filter-branchコマンド][2]を使う。フォルダ名が途中で変更されていたりする場合は、別途マージする必要がある。

```:bash
git filter-branch --subdirectory-filter {target_dir} HEAD
```

### 参考サイト

  * [SubversionのリポジトリをGitリポジトリに移行する &#8211; どっかのBlogの前身のような][3] 
      * [Subversion リポジトリを Git に移行してみるよ | バシャログ。][4] 
          * [git log で表示される画面の操作方法メモ &#8211; present][5] 
              * [Converting from Subversion to Git | Blokspeed.net][6] 
                  * [git svn 使い方メモ | Design Ambience Blog][7] 
                      * [git | git-filter-branch(1) Manual Page][2] </ul>

 [1]: http://www.amazon.co.jp/dp/427406767X
 [2]: http://git-scm.com/docs/git-filter-branch
 [3]: http://parrot.hatenadiary.jp/entry/20111025/1319519285
 [4]: http://c-brains.jp/blog/wsg/12/07/05-161309.php
 [5]: http://tnakamura.hatenablog.com/entry/20090502/1241229944
 [6]: http://blokspeed.net/blog/2010/09/converting-from-subversion-to-git/
 [7]: http://design-ambience.com/wordpress/?p=378