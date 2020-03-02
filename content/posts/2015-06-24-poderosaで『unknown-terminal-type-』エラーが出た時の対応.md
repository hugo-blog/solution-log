---
title: Poderosaで『unknown terminal type.』エラーが出た時の対応
author: tomonori
type: post
date: 2015-06-23T17:21:35+00:00
url: /?p=999
categories:
  - Poderosa

---
Windows7のPoderosaからCentOS7へSSH接続し、MariaDBへログインしようとしたら以下のメッセージが表示された。

```:bash
"kterm": unknown terminal type.```

### CentOS7の環境変数を変更する

```:bash
# 一時的に環境変数を書き換える
export TERM=linux

# 設定ファイルに書き込む
echo "export TERM=linux" &gt;&gt; ~/.bash_profile
```

### 参考サイト

  * [Resolving &#8220;&#8216;unknown&#8217;: unknown terminal type.&#8221; error &#8211; Tech Titbits][1]

 [1]: http://techtitbits.com/2010/10/resolving-unknown-unknown-terminal-type-error/