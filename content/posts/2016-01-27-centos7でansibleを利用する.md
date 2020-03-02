---
title: CentOS7でAnsibleを利用する
author: tomonori
type: post
date: 2016-01-27T08:15:14+00:00
url: /?p=1156
categories:
  - Ansible
  - CentOS7

---
### Ansibleを管理ホストへインストールする

Ansibleのインストールには管理ホスト側にPython2.6以上、対象ホスト側には2.4以上が必要。

```:bash
yum install epel-release
yum install ansible
```

### Ansibleを設定する

```:bash
vi /etc/ansible/ansible.cfg


#################################
[defaults]
host_key_checking=false
```

### 対象ホストへSSH接続する

もし接続エラーが出る場合は「**-vvv**」オプションを追加して接続したり「**rm -rf /root/.ansible/tmp/***」などでキャッシュをクリアしたりする。

```:bash
# 対象ホストを設定する
vi /etc/ansible/hosts

# This is the default ansible "hosts" file.
#
# It should live in /etc/ansible/hosts
#
#   - Comments begin with the "#" character
#   - Blank lines are ignored
#   - Groups of hosts are delimited by [header] elements
#   - You can enter hostnames or ip addresses
#   - A hostname/ip can be a member of multiple groups

example.com #←対象ホスト名


# ペア鍵を使わない場合はパスワード接続を行う
# 接続を確認する
ansible all -u root --ask-pass -m ping -vvv

# inventoryファイルを設定しない場合
ansible-playbook ansible.yml -u root -k -i "hostname," --ask-vault-pass -vvvv

# 対象サーバの状態を確認する
ansible all -u root --ask-pass -m setup -vvv
```

### 参考サイト

  * [SSH Error: Permission denied (publickey,password) in Ansible &#8211; Stack Overflow](http://stackoverflow.com/questions/33280244/ssh-error-permission-denied-publickey-password-in-ansible)
  * [AnsibleでのEC2接続時エラーへの対処法 ｜ Developers.IO](http://dev.classmethod.jp/server-side/ansible/ansible-ec2-error/)
  * [とあるSIerの憂鬱 | Minimal インストールの CentOS 6.4 に最新版の ansible をインストールする](http://d.hatena.ne.jp/incarose86/comment/20150214/1423915834)
  * [ansible-playbookコマンドをInventoryファイルなしで実行する &#8211; Qiita](http://qiita.com/ariarijp/items/503f6fdcc9ff8b35f374)</ul>