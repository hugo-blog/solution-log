---
title: CentOS6でmod_userdirを使用する
author: tomonori
type: post
date: 2013-03-12T17:15:14+00:00
url: /?p=745
categories:
  - Apache

---
動作確認は「**http://localhost/~{user}/**」にアクセスする。

### mod_userdirの設定をする

```:bash
vi /etc/httpd/conf.d/userdir.conf
UserDir public_html  # 「~/public_html」にファイルを置くことでApacheに認識される
UserDir disabled root

vi /etc/httpd/conf/httpd.conf
# mod_userdirを有効にする
&lt;IfModule mod_userdir.c&gt;
    UserDir public_html
&lt;/IfModule&gt;

# Directory /home/*/public_htmlを修正する
&lt;Directory /home/*/public_html&gt;
    AllowOverride All
    Options IncludesNoExec ExecCGI FollowSymLinks
    &lt;Limit GET POST OPTIONS&gt;
        Order allow,deny
        Allow from all
    &lt;/Limit&gt;
    &lt;LimitExcept GET POST OPTIONS&gt;
        Order deny,allow
        Deny from all
    &lt;/LimitExcept&gt;
&lt;/Directory&gt;
```

### SELinuxを無効にする

しばらくハマった。SELinuxが有効だとそもそもmod_userdirが使用できないらしい。

```:bash
vi /etc/selinux/config
# SELinuxを有効にする。
SELINUX=disabled
reboot
```

### public_htmlの所有権を変更する

想定される環境だとpublic\_htmlは700になっているので、/home/userを755に、pulbic\_htmlを755に変更しておく。

### 参考サイト

  * [ユーザーディレクトリ作成(/~ユーザー名/) &#8211; CentOSで自宅サーバー構築][1] 
      * [apacheでmod_userdir.c &#8211; BIGLOBEなんでも相談室][2] 
          * [How to get UserDir (user specific public_html) working for apache in CentOS 6 | CentOSForge.com][3] 
              * [SELinuxを無効化する | Smart -Web Magazine][4] </ul>

 [1]: http://centossrv.com/apache-userdir.shtml
 [2]: http://soudan1.biglobe.ne.jp/qa5781918.html
 [3]: http://centosforge.com/node/how-get-userdir-user-specific-publichtml-working-apache-centos-6
 [4]: http://rfs.jp/server/security/selinux01.html