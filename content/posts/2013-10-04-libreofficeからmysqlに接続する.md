---
title: LibreOfficeからMySQLに接続する
author: tomonori
type: post
date: 2013-10-04T04:25:41+00:00
url: /?p=874
categories:
  - LibreOffice

---
### 想定される環境

  * CentOS6.4 
      * LibreOffice 4.1 </ul> 
        ### JDBCドライバのインストール
        
        <pre># yum -y install mysql-connector-java
```
        
        ### LibreOfficeにJDBCドライバを認識させる
        
        LibreOffice Calcを起動
  
        ツール→オプション→LibreOffice→詳細→クラスパス
  
        [<img src="http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_2-300x270.jpg" alt="140507_libreoffice_2" width="300" height="270" class="alignnone size-medium wp-image-947" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_2-300x270.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_2.jpg 572w" sizes="(max-width: 300px) 100vw, 300px" />][1]
  
        [<img src="http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_11-300x189.jpg" alt="140507_libreoffice_1" width="300" height="189" class="alignnone size-medium wp-image-946" srcset="http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_11-300x189.jpg 300w, http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_11.jpg 906w" sizes="(max-width: 300px) 100vw, 300px" />][2]
        
        /usr/share/java/mysql-connector-java.jarを追加
        
        ### LibreOffice Baseでデータベース接続を確立する
        
        LibreOfficeのBaseを開く
  
        ファイル→新規作成→データベース
  
        既存のデータベースに接続→JDBC
  
        データソースのURL
  
        JDBC:mysql://localhost:3306/databasename
  
        JDBCドライバクラス
  
        com.mysql.jdbc.Driver
  
        テストクラスでテスト
  
        ユーザー設定
  
        次のステップは適宜
  
        これで接続できるはず
        
        ### 参考サイト
        
          * [LibreOffice OpenOffice JDBC MySQL &#8211; 適当記録wiki &#8211; Seesaa Wiki（ウィキ）][3] 
              * [MySQLとOpenOffice Baseをjdbcで繋ぐ &#8211; KRAKENBEAL RECORDS][4] 
                  * [suz-lab &#8211; blog: CentOSでTomcatからDataSourceを利用してRDS(MySQL)に接続][5] </ul>

 [1]: http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_2.jpg
 [2]: http://13.112.229.114/wordpress/wp-content/uploads/2013/10/140507_libreoffice_11.jpg
 [3]: http://wiki.livedoor.jp/mb0620/d/LibreOffice%20OpenOffice%20JDBC%20MySQL
 [4]: http://krakenbeal.blogspot.jp/2011/02/mysqlopenoffice-basejdbc.html
 [5]: http://blog.suz-lab.com/2012/10/centostomcatdatasourcerdsmysql.html