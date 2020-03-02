---
title: Windows7にGitをインストールする
author: tomonori
type: post
date: 2015-05-29T13:38:28+00:00
url: /?p=990
categories:
  - Git
  - Windwos 7

---
壮年団の写真及び動画整理の関係で、コマンドプロンプトのプログラミングコードを書く必要が出てきた。Windows機からGitが直接使えた方が作業が楽なのでWindows7にGitをインストールした。

### Git for Windowsをインストールする

[Git for Windows](https://msysgit.github.io/)をダウンロードしインストールする。

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-8_23-57-16_No-00-300x233.png" alt="SnapCrab_Git-Setup_2013-10-8_23-57-16_No-00" width="300" height="233" class="alignnone size-medium wp-image-991" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-8_23-57-16_No-00-300x233.png 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-8_23-57-16_No-00.png 513w" sizes="(max-width: 300px) 100vw, 300px" />][1]

『**Run Git from the Windows Command Prompt**』を選ぶと良い。『**Run Git and included Unix tools from the Windows Command Prompt**』は環境変数を汚染する可能性があるので使わないほうが無難。

[<img src="http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-14_15-1-32_No-00-300x233.png" alt="SnapCrab_Git-Setup_2013-10-14_15-1-32_No-00" width="300" height="233" class="alignnone size-medium wp-image-994" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-14_15-1-32_No-00-300x233.png 300w, http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-14_15-1-32_No-00.png 513w" sizes="(max-width: 300px) 100vw, 300px" />][2]

この画面は『**Checkout Windows-style, commit Unix line endings**』を選んだが、まだ運用してみないとどう出るのか分からない。

### SSHの設定を行う

Git for Windowsは『**/user/.ssh**』に鍵ファイルを探しに行くので、このフォルダに鍵ファイルを設置する。

### 参考サイト

  * [Git for Windows](https://msysgit.github.io/)</il> 
      * [私家版 Git For Windowsのインストール手順 | OPC Diary](http://opcdiary.net/?page_id=27065)</ul>

 [1]: http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-8_23-57-16_No-00.png
 [2]: http://13.112.229.114/wordpress/wp-content/uploads/2015/05/SnapCrab_Git-Setup_2013-10-14_15-1-32_No-00.png