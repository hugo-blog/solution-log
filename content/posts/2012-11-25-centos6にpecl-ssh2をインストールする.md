---
title: CentOS6にPECL::ssh2をインストールする
author: tomonori
type: post
date: 2012-11-25T14:10:02+00:00
url: /?p=183
categories:
  - CentOS6
  - PECL
  - PHP
  - SSH

---
### PECL::ssh2をインストールする

インストールにはPEARおよびlibssh2、libssh2-develが必要。適宜インストールしておく。

```:bash
yum install php-devel php-pear libssh2 libssh2-devel
pecl install ssh2 channel://pecl.php.net/ssh2-0.12
```

php.iniを開き、extension=ssh2.soを書き加える

### PECL::ssh2を使ったPHPのサンプルコード

```:php
&lt;?php

$connection = ssh2_connect("ssh.host", 22);

if (ssh2_auth_password($connection, "user", "pass")) {
	echo "Authentication Successful!\n";
} else {
	die("Authentication Failed...");
}

ssh2_scp_send($connection, "/remote/file",
"/local/file", 0644);

?&gt;
```

ファイル名はローカルとリモートで必ずフルパスで指定すること！

### 参考サイト

  * [Installing SSH2 Extension for PHP on CentOS 5 &#124; Dynamic Hosting Blog][1]

 [1]: http://blog.dynamichosting.biz/2011/01/10/installing-ssh2-extension-for-php-on-centos-5/