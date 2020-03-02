---
title: CentOS7でSeleniumをDockerコンテナで利用する
author: tomonori
type: post
date: 2017-01-28T13:33:06+00:00
url: /?p=1206
categories:
  - CentOS6
  - Python
  - Selenium

---
### DockerをインストールしFirefoxコンテナを立ち上げる

```:bash
yum install docker
systemctl start docker
sytemctl enable docker
docker run -d -p 4444:4444 selenium/standalone-firefox:latest
```

### Python Seleniumをインストールする

```:bash
yum install epel-release
yum install python-pip
pip install --upgrade pip
pip install selenium
```

### DockerコンテナのIPアドレスを取得する

接続先のDockerコンテナのIPアドレスを指定しないとコンテナに接続出来ない。テストスクリプトにIPアドレスを記述するために取得する。

```:bash
docker ps | awk "NR&gt;1&&$0=$1" | xargs -n 1 docker inspect -f "{{.Name}} {{.NetworkSettings.IPAddress }}"
```

### テストスクリプト例

```:python
from selenium import webdriver                                                               
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities               
                                                                                             
firefox_capabilities = DesiredCapabilities.FIREFOX                                           
                                                                                             
driver = webdriver.Remote(                                                                   
  command_executor="http://127.17.0.2:4444/wd/hub",                                          
  desired_capabilities=firefox_capabilities)                                                 
driver.get("http://yahoo.co.jp")                                                             
                                                                                             
print driver.title                                                                           
                                                                                             
driver.close()
```

### 参考サイト

  * [CentOS7 で Docker を使ってみる &#8211; CUBE SUGAR CONTAINER](http://blog.amedama.jp/entry/2015/09/24/222040)
  * [稼働中のdockerコンテナのIPアドレス一覧を取得する &#8211; くんすとの備忘録](http://www.kunst1080.net/entry/2016/05/29/005815)
  * [2. Getting Started — Selenium Python Bindings 2 documentation](http://selenium-python.readthedocs.io/getting-started.html)