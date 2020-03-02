---
title: CentOS6.4にLibreOfficeをインストールする
author: tomonori
type: post
date: 2013-10-01T07:09:38+00:00
url: /?p=871
categories:
  - LibreOffice

---
lenovoノートPCのVMWare Player上でゲストOSとして稼働しているCentOS6.4にLibreOfficeをインストールしたときのメモ

### パッケージをダウンロードする

[LibreOfficeのダウンロードページ][1]から必要なパッケージをダウンロード。

今回は

  * メイン・インストールパッケージ 
      * 言語パッケージ 
          * ヘルプパッケージ </ul> 
            をダウンロード。
            
            ### メインパッケージをインストールする
            
            ```:bash
tar zxvf LibreOffice_4.1.1_Linux_x86_rpm.tar.gz
cd LibreOffice_4.1.1_Linux_x86_rpm.tar.gz/RPMS
rpm -Uvh *.rpm
```
            
            ### 言語パッケージをインストールする
            
            ```:bash
tar zxvf LibreOffice_4.1.1_Linux_x86_rpm_langpack_ja.tar.gz
cd LibreOffice_4.1.1.2_Linux_x86_rpm_langpack_ja/RPMS
rpm -Uvh *.rpm
```
            
            ### LibreOffice ヘルプパッケージをインストールする
            
            ```:bash
tar zxvf LibreOffice_4.1.1.2_Linux_x86_rpm_helppack_ja
cd LibreOffice_4.1.1.2_Linux_x86_rpm_helppack_ja/RPMS
rpm -Uvh *.rpm
```
            
            ### 参考サイト
            
              * [CentOS6.2にLibreOfficeのインストール &laquo; MARU's Blog][2] </ul>

 [1]: http://ja.libreoffice.org/download/
 [2]: http://www.maruweb.jp.net/wp/?p=653