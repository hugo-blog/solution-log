---
title: CentOS6.3にWordPressをインストールする
author: tomonori
type: post
date: 2012-12-31T08:02:40+00:00
url: /?p=463
categories:
  - CentOS6
  - WordPress

---
権限の設定でハマったのでメモ。

### WordPressをダウンロード、解凍する

最新版は[こちら][1]から確認する。

```:bash
cd /path/to/wordpressfolder
wget http://ja.wordpress.org/wordpress-3.5-ja.zip
unzip wordpress-3.5-ja.zip  #wordpressと言うフォルダに展開される
mv wordpress blogfolder
```

### データベースを作成する

rootですべてやっても良いけど、危険なので一応WordPress用のデータベースユーザーを作成し対応する

```:bash
mysql -u root -p #パスワードを入力
CREATE DATABASE wordpress-db;
GRANT ALL ON wordpress-db.* TO wpuser@localhost IDENTIFIED BY 'パスワード';
```

### WordPressの権限を書き換える

CentOSでユーザを作成し、そのユーザに権限を与えるのかと思ったが、どうやらApacheに権限を与えるらしい。。。

```:bash
chown -R apache:apache blogfolder
```

### WordPressの初期設定を行いブログを開設する

上記の設定が済んだらWordPressのトップページにアクセスしてブログを開設する

### 参考サイト

  * [ブログサイト構築(WordPress) -CentOSで自宅サーバー構築][2] </ul>

 [1]: http://ja.wordpress.org/
 [2]: http://centossrv.com/wordpress.shtml