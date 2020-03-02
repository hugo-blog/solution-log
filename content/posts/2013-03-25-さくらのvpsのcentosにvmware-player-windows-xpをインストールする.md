---
title: さくらのVPSのCentOSにVMWare Player + Windows XPをインストールする
author: tomonori
type: post
date: 2013-03-25T01:42:01+00:00
url: /?p=787
categories:
  - CentOS6
  - さくらのVPS

---
### VMWare Playerをインストールする

https://my.vmware.com/web/vmware/free#desktop\_end\_user\_computing/vmware\_player/5_0

さくらのVPSからVMWare Playerを使うにはバイバスを通す必要がある
  
http://communities.vmware.com/docs/DOC-8970
  
vmx.allowNested = TRUE

ローカルのCentOSでEpson LP-S2000を使う場合は、ドライバをインストールした後、一度電源を入れて認識させ、プリンタを削除してもう一度接続し直す。

### VMWare Playerをアンインストールする

```:bash
/usr/bin/vmware-installer -u vmware-player
```

### 参考サイト

  * [VMware Communities: VMware Player connot run on this CPU][1] 
      * [VMware Communities: Running Nested VMs][2] 
          * [Uninstall Vmware Player][3]
  
            </u></p>

 [1]: http://communities.vmware.com/message/2000733
 [2]: http://communities.vmware.com/docs/DOC-8970
 [3]: http://www.netdip.com/uninstall-vmware-player-20d/