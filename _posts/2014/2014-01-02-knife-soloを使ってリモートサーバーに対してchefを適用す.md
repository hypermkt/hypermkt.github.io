---
layout: post
title: knife-soloを使ってリモートサーバーに対してChefを適用する
post_id: 45
categories: 
- Chef
- Vagrant
---

[![Chef Logo](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/6521.OC_Chef_Logo-300x236.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/6521.OC_Chef_Logo-e1388648008381.png)


## 概要


Chef-soloはサーバーにログインして実行する必要がありますが、管理するサーバー台数が増えてくると各サーバーに都度ログインしてChef Soloを実行するのが面倒くさくなりました。そこで管理サーバーとして大本のサーバーから各サーバーに対してChef Soloが実行できるようにし、手間を減らします。


## 準備


デモなのでVagrantを使用します。同じ事を本番サーバーでも可能です。


*  Vagrant 1.3.5


*  Box:CentOS 5.8 x64


## 前提



*  ruby 1.9.3がインストール済み


*  manageからwebに対して鍵無しログインが可能


## Vagrantfile


knife soloを実行する管理サーバー(manage)とChef Soloを適用するウェブサーバー(web1)を起動します。


# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "manage" do |manage|
    manage.vm.box = "CentOS-5.8-x86_64"
    manage.vm.hostname = "manage"
    manage.vm.network :private_network, ip: "192.168.33.10"
    manage.vm.network :public_network
  end

  config.vm.define "web" do |web|
    web.vm.box = "CentOS-5.8-x86_64"
    web.vm.hostname = "web"
    web.vm.network :private_network, ip: "192.168.33.20"
    web.vm.network :public_network
  end
end


## knife-soloのインストール



$ gem install knife-solo


## cookboosのひな形を準備



[vagrant@manage ~]$ knife solo init knife_chef
Creating kitchen...
Creating knife.rb in kitchen...
Creating cupboards...


## httpdをインストールするとrecipeを準備



[vagrant@manage knife_chef]$ knife cookbook create httpd -o cookbooks
*  * Creating cookbook httpd
*  * Creating README for cookbook: httpd
*  * Creating CHANGELOG for cookbook: httpd
*  * Creating metadata for cookbook: httpd
[vagrant@manage knife_chef]$ vim cookbooks/httpd/recipes/default.rb 
package "httpd" do
  action :install
end


## リモートサーバーでChef-soloが実行できるように準備



$ knife solo prepare vagrant@192.168.33.20
[vagrant@manage ~]$ knife solo prepare vagrant@192.168.33.20
WARNING: No knife configuration file found
Bootstrapping Chef...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 14101  100 14101    0     0  14009      0  0:00:01  0:00:01 --:--:-- 69806
Downloading Chef 10.12.0 for el...
downloading https://www.opscode.com/chef/metadata?v=10.12.0&prerelease=false&p=el&pv=5&m=x86_64
  to file /tmp/install.sh.9830/metadata.txt
trying wget...
url https://opscode-omnibus-packages.s3.amazonaws.com/el/5/x86_64/chef-10.12.0-1.el5.x86_64.rpm
md5 25887ebd80bbb488b88b9360a837d736
sha256  6047a88dd52d9bf33e4c30de36c57fa7308477e73cec07f1703eaba1e1bb366a
downloaded metadata file looks valid...
downloading https://opscode-omnibus-packages.s3.amazonaws.com/el/5/x86_64/chef-10.12.0-1.el5.x86_64.rpm
  to file /tmp/install.sh.9830/chef-10.12.0.x86_64.rpm
trying wget...
Checksum compare with sha256sum succeeded.
Installing Chef 10.12.0
installing with rpm...
Preparing...                ### ### ### ### ### ### ### ### ### ### ### ### ### ### # [100%]
   1:chef                   ### ### ### ### ### ### ### ### ### ### ### ### ### ### # [100%]
Thank you for installing Chef!
Generating node config 'nodes/192.168.33.20.json'...


## nodeファイルにhttpdをインストールするrecipeを追加



[vagrant@manage knife_chef]$ vim nodes/192.168.33.20.json 
{
  "run_list":[
    "recipe[httpd::default]"
  ]
}


## knife-soloを実行



[vagrant@manage knife_chef]$ knife solo cook vagrant@192.168.33.20
Running Chef on 192.168.33.20...
Checking Chef version...
Uploading the kitchen...
Generating solo config...
Running Chef...
Starting Chef Client, version 11.8.2
Compiling Cookbooks...
Converging 1 resources
Recipe: httpd::default
  * package[httpd] action install
    - install version 2.2.3-83.el5.centos of package httpd

Chef Client finished, 1 resources updated


## リモート先のサーバーで確認



### knife-solo実行前



[vagrant@web ~]$ yum list installed | grep httpd
[vagrant@web ~]$


### knife-solo実行後


リモート先に遠隔越しにインストールされました


[vagrant@web ~]$ yum list installed | grep httpd
httpd.x86_64                      2.2.3-83.el5.centos                  installed

サーバー一台ならChef soloで十分なんですが複数台となるとログインし直すが大変面倒なので管理サーバーなどからKnife solo cookコマンド一発でリモート先を変更できるようになるので楽ですねー！