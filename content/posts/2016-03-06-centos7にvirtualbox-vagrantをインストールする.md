---
title: CentOS7にVirtualBox + vagrantをインストールする
author: tomonori
type: post
date: 2016-03-06T11:34:43+00:00
url: /?p=1174
categories:
  - CentOS7
  - vagrant
  - Virtual Box

---
### VirtualBoxをインストールする

[こちらのサイト](http://qiita.com/Itomaki/items/9a6a314a853cdcd00f80)を参考にした。

```:bash
yum istall dkms

cd /etc/yum.repos.d/
sudo wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo

yum update

yum list | grep VirtualBox

yum install VirtualBox-xxxx
```

### Vagrantをインストールする

[インストール](http://blog.amedama.jp/entry/2015/08/19/204044)しておく。

```:bash
# Vagrantをインストールする
rpm -Uvh https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.rpm

# Guest Additions を自動インストールする
vagrant plugin install vagrant-vbguest
```

### BIOSの設定を変更する

PCを立ち上げてF10でBIOSの設定画面を表示させ仮想化技術を有効にする

### Vagrantfileの設定を変更する

開発用途では使用しないUSB2.0を無効にする。またエラーメッセージを広いやすくするためにメッセージを表示する設定にする

```:bash
vagrant pull &lt;box-image&gt;
vagrant init &lt;box-name&gt;
vi Vagrantfile 

-*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don"t change it unless you know what
# you"re doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,# accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 22, host: 2223

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true

    # Customize the amount of memory on the VM:
    # vb.memory = "1024"
    
    # Disable USB 2.0
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--usbehci", "off"]
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: &lt;&lt;-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL

```

### 参考サイト

  * [Vagrant: vagrant-vbguest プラグインで仮想マシンの Guest Additions を最新に保つ &#8211; CUBE SUGAR CONTAINER](http://blog.amedama.jp/entry/2015/08/19/204044)
  * [仮想化支援機構(VT-x/AMD-V)を有効化できません | Futurismo](http://futurismo.biz/archives/1647)
  * [Box does not Boot Up · Issue #3 · jonschipp/vagrant](https://github.com/jonschipp/vagrant/issues/3)