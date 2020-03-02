---
title: CentOS6.3でFileZillaを使用する
author: tomonori
type: post
date: 2012-12-04T01:32:06+00:00
url: /?p=306
categories:
  - CentOS6
  - FileZilla

---
### FileZillaをインストールする

最初は[gFTP][1]を[RPMからインストール][2]しようと思ったけど、[依存関係でシステムがイカれるかも][3]とか言う書き込みを見てしまったし、他のサイトの記事を見ても非常に面倒くさそうだったので、yumから[FileZilla][4]をインストールすることにした。

```:bash
yum --enablerepo=epel list install filezilla #パッケージがあるか確認
yum --enablerepo=epel install filezilla #インストール
```

インストールが完了すると、CentOS6のツールバーからアプリケーション→インターネット→FileZillaで立ち上がる。ターミナルからは立ち上げないと思うのでとりあえずはこれで良しとしておく。

### FileZillaを設定する

基本的にはFFFTPとそんなに変わらない感じ。FileZillaの上部になるホスト、ユーザー名、パスワードの欄に必要事項を記入してクイック接続をクリック。ポートは設定しなくても、FileZilla側が接続時に勝手に見つけてくれる感じだ。

また一回接続すると、接続履歴が残るので次回からは簡単に目的のサーバにFTP接続できるようになる。

### 参考サイト

  * [CentOS6.2 FileZillaのインストール | SlackHack][5]

 [1]: http://gftp.seul.org/
 [2]: http://ja.528p.com/linux/client6/CD004-gftp-settei.html
 [3]: http://serverfault.com/questions/116686/how-to-install-glib-in-a-centos-server
 [4]: http://filezilla-project.org/
 [5]: http://slackhack.net/?p=282