---
title: CentOS6のApacheでmod_cacheを設定する
author: tomonori
type: post
date: 2013-03-18T11:50:45+00:00
url: /?p=770
categories:
  - Apache
  - CentOS6

---
Redmineが若干早くなるかもしれないのでmod_cacheを設定してみた。

### mod_cacheのインストール確認を行う

想定される環境だと**mod_cache**および**mod\_disk\_cache**は既にインストール済み。

```:bash
httpd -M | grep cache
Syntax OK
cache_module (shared)
disk_cache_module (shared)
```

### mod_cahceの設定を行う

「**/etc/httpd/conf.d/cache.conf**」にモジュールの設定を書く。[マニュアル][1]によると「**CacheDirLevelsの値 X CacheDirLengthの値**」は「**20**」以下でなければならないので注意。

```:bash
vi /etc/httpd/conf.d/cache.conf

# 以下を書き込む
# LoadModule cache_module modules/mod_cache.so
# LoadModule disk_cache_module modules/mod_disk_cache.so

&lt;IfModule mod_cache.c&gt;
    &lt;IfModule mod_disk_cache.c&gt;
        # リクエストの Cache-Control: no-cache ヘッダを無視してキャッシュを返す
        CacheIgnoreCacheControl On

        # レスポンスにLast-Modifiedヘッダがなくてもキャッシュ対象にする
        CacheIgnoreNoLastMod On

        # Set-Cookie ヘッダをキャッシュしない
        CacheIgnoreHeaders Set-Cookie

        # キャッシュデータの保存先
        CacheRoot /var/cache/apache

        # キャッシュ対象とするパスの指定
        CacheEnable disk /

　　　　# キャッシュデータを保管するディレクトリの階層の深さ
        CacheDirLevels 5

        # キャッシュデータを保管するディレクトリの文字数
        CacheDirLength 3
    &lt;/IfModule&gt;
&lt;/IfModule&gt;

# キャッシュ保管ディレクトリの権限を変更しておく
mkdir /var/cache/apache
chown apache:apache /var/cache/apache

# Apacheを再起動する
service httpd restart
```

### 参考サイト

  * [Apache の mod_cache で日記の高速化に挑戦してみた &#8211; まちゅダイアリー(2010-06-26)][2] 
      * [mod\_disk\_cache &#8211; Apache HTTP サーバ][1] </ul>

 [1]: http://httpd.apache.org/docs/2.2/ja/mod/mod_disk_cache.html
 [2]: http://www.machu.jp/diary/20100626.html#p01