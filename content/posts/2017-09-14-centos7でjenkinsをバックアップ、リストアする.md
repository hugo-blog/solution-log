---
title: CentOS7でJenkinsをバックアップ、リストアする
author: tomonori
type: post
date: 2017-09-14T13:29:22+00:00
url: /?p=1320
categories:
  - CentOS7
  - Jenkins

---
### Jenkinsバックアップデータを作成する

```:bash
# Jenkins稼働サーバで
systemctl stop jenkins

git clone https://github.com/sue445/jenkins-backup-script.git
bash jenkins-backup.sh /path/to/jenkins archives.tar.gz
```

### Jenkinsをリストアする

```:bash
# Jenkinsを停止する
systemctl stop jenkins

# 稼働中のJenkinsのデータを消去する
rm -rf /var/cache/jenkins/*
rm -rf /path/to/jenkins/jobs/
rm -rf /path/to/jenkins/plugins/
rm -rf /path/to/jenkins/users/
rm -rf /path/to/jenkins/secrets/

# バックアップデータをリストアする
tar xfvz archives.tar.gz
cd jenkins-backup/
mv -f * /path/to/jenkins
chown -R jenkins:jenkins /path/to/jenkins

# Jenkinsを再起動する
systemctl start jenkins
```

### 参考サイト

#### バックアップ

  * [Jenkinsのバックアップ方法 &#8211; Qiita](http://qiita.com/tnishi91/items/d2ba5c42408333eeca9a)
  * [GitHub &#8211; sue445/jenkins-backup-script: archive jenkins setting and plugins](https://github.com/sue445/jenkins-backup-script)

#### リストア

  * [cpコマンド で確認なしに上書き（Linux） &#8211; 創屋ぷれす](http://www.souya.biz/blog/2009/08/01/cp%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%A7%E7%A2%BA%E8%AA%8D%E3%81%AA%E3%81%97%E3%81%AB%E4%B8%8A%E6%9B%B8%E3%81%8D%EF%BC%88linux%EF%BC%89/)

#### Redmineチケット

  * [Jenkinsのバックアップとリストアについて調べる](/redmine/issues/325)