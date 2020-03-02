---
title: CentOS7でmod_userdirを使用する
author: tomonori
type: post
date: 2015-11-16T15:38:58+00:00
url: /?p=1075
categories:
  - Apache
  - CentOS7

---
### mod_userdirの設定をする

```:bash
vi /etc/httpd/conf.d/userdir.conf
#
# UserDir: The name of the directory that is appended onto a user"s home
# directory if a ~user request is received.
#
# The path to the end user account "public_html" directory must be
# accessible to the webserver userid.  This usually means that ~userid
# must have permissions of 711, ~userid/public_html must have permissions
# of 755, and documents contained therein must be world-readable.
# Otherwise, the client will only receive a "403 Forbidden" message.
#
&lt;IfModule mod_userdir.c&gt;
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory
    # permissions).
    #
    UserDir disabled
    UserDir enabled user

    #
    # To enable requests to /~user/ to serve the user"s public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    UserDir public_html
&lt;/IfModule&gt;

#
# Control access to UserDir directories.  The following is an example
# for a site where these directories are restricted to read-only.
#
&lt;Directory "/home/*/public_html"&gt;

    Options IncludesNoExec ExecCGI FollowSymLinks
    Require all granted
&lt;/Directory&gt;

```

### SELinuxを無効にする

SELinuxでmod_userdirを許可する設定も可能だが今回はSELinuxを無効にする

```:bash
vi /etc/selinux/config
# SELinuxを無効にする。
SELINUX=disabled
reboot

# Apacheを起動する
systemctl restart httpd.service
systemctl enable httpd.service
```

### public_htmlの所有権を変更する

想定される環境だとpublic\_htmlは700になっているので、/home/userを711に、pulbic\_htmlを755に変更しておく。

### 参考サイト

  * [mod_userdir &#8211; Apache HTTP サーバ バージョン 2.2](http://httpd.apache.org/docs/2.2/ja/mod/mod_userdir.html)
  * [◇ユーザーディレクトリの設定◇初心者のためのLinuxサーバー構築講座(CentOS 自宅サーバー対応)☆お便利サーバー.com☆](http://www.obenri.com/_webserver/user_directory.html)