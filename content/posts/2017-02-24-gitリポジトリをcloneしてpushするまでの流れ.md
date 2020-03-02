---
title: Gitリポジトリをcloneしてpushするまでの流れ
author: tomonori
type: post
date: 2017-02-24T13:48:06+00:00
url: /?p=1231
categories:
  - Git

---
### クライアント側の設定をする

クライアント側では最低限「**user.name**」および「**user.email**」を設定しておく。

```:bash
# 現在の設定を確認する
git config --list

# ユーザー名、メールアドレスを適宜設定する
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

### リポジトリをCloneする

SSHで接続する場合。

```:bash
git clone ssh://{{ user }}@{{ yourdomain }}:{{ port }}/var/git/{{ git-repo }}
```

### リポジトリに編集をPushする

```:bash
# 変更したファイルをステージングする
git add -A

# ファイルの状態を確認する
git status

# 変更をコミットする
git commit -m "ファイルを変更した旨のコメント"

# リモートブランチへpushする
git push origin master
```

### 参考サイト

  * [Git &#8211; Git の設定](https://git-scm.com/book/ja/v1/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%81%AE%E8%A8%AD%E5%AE%9A)
  * [Git &#8211; 変更内容のリポジトリへの記録](https://git-scm.com/book/ja/v1/Git-%E3%81%AE%E5%9F%BA%E6%9C%AC-%E5%A4%89%E6%9B%B4%E5%86%85%E5%AE%B9%E3%81%AE%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%B8%E3%81%AE%E8%A8%98%E9%8C%B2)