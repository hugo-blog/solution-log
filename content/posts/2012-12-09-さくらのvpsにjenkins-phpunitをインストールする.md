---
title: さくらのVPSにJenkins + PHPUnitをインストールする
author: tomonori
type: post
date: 2012-12-09T03:58:08+00:00
url: /?p=402
categories:
  - CentOS6
  - Jenkins
  - PHPUnit
  - さくらのVPS

---
さくらのVPSにJenkins環境を整えるメモ。

### Xdebugをインストールする

PECLからインストールする

```:bash
pecl install xdebug
```

### PHPUnitをインストールする

**/usr/share/pear/PHPUnit**にインストールされる。インストールコマンドは良く変更されるので[PHPUnitのHP][1]を良く確認する。

```:bash
pear config-set auto_discover 1
pear install pear.phpunit.de/PHPUnit
```

### phpDocumentorをインストールする

[phpDocumentorの公式サイト][2]を参考にする。**/usr/share/pear/PhpDocumentor**にインストールされる

```:bash
pear channel-discover pear.phpdoc.org
pear install phpdoc/phpDocumentor-alpha
```

### その他のライブラリをインストールする

```:bash
pear channel-discover pear.phpmd.org
pear channel-discover pear.symfony-project.com
pear channel-discover pear.symfony.com
pear channel-discover pecl.php.net
pear channel-discover pear.pdepend.org
pear channel-discover pear.netpirates.net
pear install --alldeps phpunit/phpcpd
pear install --alldeps phpmd/PHP_PMD # PHPコードの妥当性を調べるライブラリ
```

### 参考サイト

  * [CakePHP2.0+Jenkinsで継続的インテグレーションを行う方法 | Ryuzee.com][3] 
      * [CakePHP2.X+PHPUnit+jenkinsでテストを自動化する | ミラボ][4]

 [1]: http://www.phpunit.de/manual/3.8/ja/installation.html
 [2]: http://www.phpdoc.org/
 [3]: http://www.ryuzee.com/contents/blog/5645
 [4]: http://log.miraoto.com/2012/08/656/