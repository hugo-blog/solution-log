---
title: CentOS7でRedmineをバックアップ、リストアする
author: tomonori
type: post
date: 2017-09-14T12:30:57+00:00
url: /?p=1312
categories:
  - CentOS7
  - Redmine

---
### Redmineをバックアップをする

```:bash
# 作業用ディレクトリへcdする
cd /tmp

# Gitリポジトリをアーカイブする
tar cfv git.tar /var/git/

# Redmineメディアファイルをアーカイブする
tar cfv files.tar /var/lib/redmine/files/

# PostgeSQLデータベースをダンプする
pg_dump -h localhost -U redmine redmine &gt; redmine.psql

# アーカイブデータ、データベースダンプデータをアーカイブする
tar cfv redmine.bk.tar *

# アーカイブスデータを公開ディレクトリへ移動させる
mv redmine.bk.tar /var/www/html/
```

### Redmineをリストアする

```:bash
# 作業用ディレクトリへcdする
cd /tmp

# Redmineバックアップアーカイブスデータを取得する
curl -O http://example.com/redmine.bk.tar

# アーカイブスデータを解凍する
tar xfv redmine.bk.tar

# Gitリポジトリを配置する
cd /tmp/redmine
mv git /var/

# メディアファイルを配置する
cd /tmp/redmine/
mv files/ /var/lib/redmine/

# PostgreSQLデータベースをリストアする
systemctl stop postgresql.service
systemctl start postgresql.service
sudo -u postgres dropdb redmine;
sudo -u postgres createdb -E UTF-8 -l ja_JP.UTF-8 -O redmine -T template0 redmine
psql -h localhost -U redmine redmine &lt; /tmp/redmine/redmine.psql 

# Redmineのセッションデータをクリアする
cd /var/lib/redmine
bundle exec rake db:migrate RAILS_ENV=production
bundle exec rake tmp:cache:clear tmp:sessions:clear RAILS_ENV=production

# httpdを再起動する
systemctl stop httpd.service
systemctl start httpd.service
```