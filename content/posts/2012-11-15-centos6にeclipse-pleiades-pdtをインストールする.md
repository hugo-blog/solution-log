---
title: CentOS6にEclipse + Pleiades + PDTをインストールする
author: tomonori
type: post
date: 2012-11-15T06:33:56+00:00
url: /?p=157
categories:
  - CentOS6
  - Eclipse

---
### Eclipseをインストールする

  1. <http://www.eclipse.org/downloads/>より「**Eclipse Classic 4.2.1**」を選択しブラウザでダウンロード
  2. 「**~/ダウンロード**」にダウンロードされるのでそのフォルダで解凍する
  3. Eclipse配置用ディレクトリを作成する。今回は「**~/アプリケーション**」以下にEclipseを配置する。 
      * mvコマンドで「**~/アプリケーション**」へ移動させる</ol> 
    ```:bash
cd ~/ダウンロード
tar xvf eclipse-SDK-4.2.1-linux-gtk.tar.gz
mv eclipse ~/アプリケーション
```
    
    ### Pleiadesをインストールする
    
    pleiadesのZIPアーカイブは、フォルダなしで展開されるので、事前に展開するフォルダを作成しておく。
    
    ```:bash
mkdir ~/ダウンロード/pleiades
unzip pleiades.zip -d ~/ダウンロード/pleiades/
cp -a ~/ダウンロード/pleiades/* ~/アプリケーション/eclipse/
```
    
    『**~/アプリケーション/eclipse/eclipse.ini**』の最終行に設定を記述する
    
    ```:text
-javaagent:/home/{user}/アプリケーション/eclipse/plugins/jp.sourceforge.mergedoc.pleiades/pleiades.jar
```
    
    *この記述は絶対パスで指定しないとシンボリックリンクでEclipseを立ち上げるとき、Eclipseがpleaiades.jarを見つけられずにエラーとなる。
    
    ### シンボリックリンクを作成する
    
    デスクトップにショートカットアイコンを作成し、/usr/binにもリンクを通す
    
    ```:bash
cd ~/デスクトップ/
ln -s ~/アプリケーション/eclipse/eclipse eclipse
ln -s ~/アプリケーション/eclipse/eclipse /usr/local/bin
```
    
    これでデスクトップからショートカットアイコンで起動でき、尚かつターミナルから**「eclipse」**と入力すればEclipseが立ち上がるようになる。
    
    ### PDT(PHP Developing Tools)をインストールする
    
    <http://wiki.eclipse.org/PDT/Installation_3.1.x>
  
    基本的には上記URLの通りに入れる
    
    ### 参考サイト
    
      * [Mac版 Eclipse4.2の日本語化手順 &#8211; いっぽんの猟銃のむこうに (DAIZOじいさんとGun)][1]
      * [PDT/Installation 3.1.x &#8211; Eclipsepedi][2]

 [1]: http://d.hatena.ne.jp/tkizm/20120701/1341121980
 [2]: http://wiki.eclipse.org/PDT/Installation_3.1.x