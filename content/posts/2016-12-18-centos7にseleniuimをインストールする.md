---
title: CentOS7にSeleniuimをインストールする
author: tomonori
type: post
date: 2016-12-18T14:52:57+00:00
url: /?p=1201
categories:
  - CentOS7
  - Python
  - Selenium

---
### Pipをインストールする

```:bash
yum install epel-release
yum install python-pip
pip install --upgrade pip
```

### Xvfbをインストールする

ブラウザを立ち上げずにSeleniumを操作するのに必要なライブラリをインストールする

```:bash
yum -y install xorg-x11-server-Xvfb
pip install pyvirtualdisplay
```

### SeleniumとFirefoxをインストールする

SeleniumからFirefoxを操作するにはGeckodriverをインストールする。他のブラウザを利用する時でも何らかのドライバをインストールする必要がありそうだ。

```:bash
curl -O -L https://github.com/mozilla/geckodriver/releases/download/v0.11.1/geckodriver-v0.11.1-linux64.tar.gz
tar fzxv geckodriver-v0.11.1-linux64.tar.gz 
mv geckodriver /usr/local/bin/

pip install selenium

yum install firefox
```

### テストスクリプト例

CLI環境でスクリプトを実行する場合は必ずVirtualDisplayを経由させる。geckodriverがエラーで立ち上がらない。

```:python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities

from pyvirtualdisplay import Display
display = Display(visible=0, size=(1024, 768))
display.start()

firefox_capabilities = DesiredCapabilities.FIREFOX
firefox_capabilities["marionette"] = True
firefox_capabilities["binary"] = "/usr/bin/firefox"


driver = webdriver.Firefox(capabilities=firefox_capabilities)
driver.get("http://yahoo.co.jp")
print driver.title

driver.close()
```

### 参考サイト

  * [Ubuntu16.04のFirefox47でSelenium WebDriverを動かす方法 &#8211; Qiita](http://qiita.com/tukiyo3/items/003eba922c9ab8bfe3ac)
  * [Can Selenium Webdriver open browser windows silently in background? &#8211; Stack Overflow](http://stackoverflow.com/questions/16180428/can-selenium-webdriver-open-browser-windows-silently-in-background)
  * [Xvfb を使って仮想ディスプレイを作る &#8211; CUBE SUGAR CONTAINER](http://blog.amedama.jp/entry/2016/01/03/115602)