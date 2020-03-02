---
title: Windows7 + XAMPPでPerlを使う
author: tomonori
type: post
date: 2012-11-12T11:31:50+00:00
url: /?p=118
categories:
  - Eclipse
  - Perl
  - XAMPP

---
### EclipseにEPIC &#8211; Eclipse Perl Integrationをインストールする

  1. 「ヘルプ」→「新規ソフトウェアのインストール」→「使用可能なソフトウェアサイト」
  2. EPICと入力しフィルタをかける
  3. 作業対象から「EPIC &#8211; Eclipse Perl Integration &#8211; http://e-p-i-c.sf.net/updates」を選択
  4. チェックボックスにチェックを入れ次へをクリック

### 環境変数を通す

  1. 「コントロールパネル」→「システム」→「システムの詳細設定」を選択し、表示されたウィンドウの「環境変数」ボタンをクリック
  2. 変数の一覧に「PATH」がない場合は「新規」を選択し、変数名に「PATH」、変数値に「C:\www\php-5.2.9-Win32」など実行ファイルがある階層を入力。
  
    変数の一覧に「PATH」がある場合は「PATH」を選択した後「編集」を選択し、上記のように変数値を入力。

### Perlスクリプトを作成する

ファイルの拡張子を.plにし、コンテンツタイプをしっかりと指定する。

```:perl
#!C:\xampp\perl\bin\perl.exe
print "Content-type: text/html\n\n";
print "Hello World.";
```