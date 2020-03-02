---
title: CentOSで6.3でを起動時にNum Lockを有効にする
author: tomonori
type: post
date: 2013-03-09T16:37:57+00:00
url: /?p=729
categories:
  - CentOS6

---
CentOSを立ち上げたときにいつもキーボードのNum Lockが有効になっておらず面倒なので対応する。

```:bash
vi /etc/rc.d/rc.sysinit
# 以下を末尾に追加する
for tty in /dev/tty[1-9]*; do
setleds -D +num &lt; $tty
done
```

### 参考サイト

  * [centosでnumlockを起動時に有効にする][1] 
      * [Linuxシステムの起動スクリプト(inittab、rc.sysinit、rc)を解読][2] </ul> 
        http://pkgs.org/centos-6-rhel-6/repoforge-x86\_64/numlockx-1.1-1.el6.rf.x86\_64.rpm.html

 [1]: http://paki2.blog110.fc2.com/blog-entry-414.html
 [2]: http://www.geocities.jp/sugachan1973/doc/funto76.html