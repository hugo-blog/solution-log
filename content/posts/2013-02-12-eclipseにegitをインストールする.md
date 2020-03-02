---
title: EclipseにEgitをインストールする
author: tomonori
type: post
date: 2013-02-12T08:07:33+00:00
url: /?p=674
categories:
  - Eclipse
  - Git

---
少し前からEgitを使っているが、ユーザー毎のSSHの設定でかなりハマったのでメモ。

### EclipseにEgitをインストールする

WindowsでEclipseを使う場合は**Aptanaプラグインの使用時に注意する**。EclipseのAptanaのGitドライバの関係で、Gitのコミットメッセージが文字化けする場合がある。

  1. **Eclipse**→**ヘルプ**→**新規ソフトウェアのインストール**→**作業対象(W)**:から**「すべての使用可能なサイト」**を選択 
      * フィルター入力に**「Egit」**と入力 
          * すべての選択肢にチェックを入れインストールする </ol> 
            ### SSHの設定をする
            
            2時間ほどハマった。ディフォルトだとEclipseが鍵ファイルをうまくみつけられないので.ssh/configに設定を書き込む
            
            ```:bash
Host hoge # 接続名（なんでもよい）
HostName {yourhostname} # 接続先のドメイン名やIPアドレス
User {sshuser} # SSH接続ユーザー名
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa # 鍵ファイルの場所
```
            
            Eclipse側ではディフォルトで鍵ファイルを見つけに行く設定が
  
            **Eclipse**→**ウィンドウ**→**設定**→**一般**→**ネットワーク接続**→**SSH2**
  
            にある。適切に設定されているか確認する。
            
            ### 参考サイト
            
              * [ssh 接続を簡単にする ~/.ssh/config | dogmap.jp][1] 
                  * [Git &#8211; rails開発してたらコミットコメントが文字化けた話 &#8211; Qiita](http://qiita.com/shin1988/items/90cd428b5a3b64560e8b)</il> 
                      * [Ktouth Brand. on Web &#8211; 解決：Aptana(eclipse) + EGit のコミットログ文字化けと化けたログの修正方法](http://www.k-brand.gr.jp/log/002510)</il> </ul>

 [1]: http://dogmap.jp/2011/10/27/ssh_config/