---
title: CentOS6にChromiumをインストールする
author: tomonori
type: post
date: 2013-09-03T06:16:58+00:00
url: /?p=854
categories:
  - Chromium

---
CentOSでのChromeのサポートが切れてしまったのでChromiumに乗り換える

### 想定される環境

  * CentOS6.4 32bit </ul> 
    **CentOS6.3以前ではChromiumが立ち上がらないので注意!!!**
    
    ### Chromium YUM リポジトリを有効にする
    
    ```:bash
wget http://people.centos.org/hughesjr/chromium/6/chromium-el6.repo -O /etc/yum.repos.d/chromium-el6.repo
```
    
    ### Chromiumをインストールする
    
    ```:bash
yum install chromium
```
    
    ### 参考サイト
    
      * [CentOS 6.2 に Chrome をインストールする][1] </ul>

 [1]: http://kaworu.jpn.org/kaworu/2012-02-10-1.php