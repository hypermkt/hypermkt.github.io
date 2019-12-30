---
layout: post
title: CentOS5.8にyumでgitをインストールする
post_id: 26
categories: 
- Centos
- Git
---

[![Git-Logo-1788C](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/Git-Logo-1788C-300x125.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/Git-Logo-1788C-e1388646507771.png)

CentOS 5.8(Final)にyumでgitをインストールしようとするとNo package git available.と言われインストール出来ない。


[vagrant@vagrant-c5-x86_64 ~]$ sudo yum install git
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirrors.grandcloud.cn
 * extras: ftp.riken.jp
 * updates: mirrors.grandcloud.cn
base                                                     | 1.1 kB     00:00
base/primary                                             | 1.3 MB     00:06
base                                                     3662/3662
extras                                                   | 2.1 kB     00:00
extras/primary_db                                        | 173 kB     00:01
updates                                                  | 1.9 kB     00:00
updates/primary_db                                       | 197 kB     00:04
Setting up Install Process
No package git available.
Nothing to do

gitはepelリポジトリに含まれているため、まずはepelリポジトリをyumで使えるようにします。


## 環境



*  Vagrant 1.3.5


*  box:
[CentOS 5.8 x86_64](https://dl.dropbox.com/u/17738575/CentOS-5.8-x86_64.box)


## epelリポジトリのインストール



[vagrant@vagrant-c5-x86_64 ~]$ sudo rpm -ivh http://ftp.jaist.ac.jp/pub/Linux/Fedora/epel/5/x86_64/epel-release-5-4.noarch.rpm
http://ftp.jaist.ac.jp/pub/Linux/Fedora/epel/5/x86_64/epel-release-5-4.noarch.rpm を取得中
警告: /var/tmp/rpm-xfer.xRs7dj: ヘッダ V3 DSA signature: NOKEY, key ID 217521f6
エラー: stat /vagrant に失敗しました: そのようなファイルやディレクトリはありません
準備中...                ### ### ### ### ### ### ### ### ### ### ### ### ### ### # [100%]
   1:epel-release           ### ### ### ### ### ### ### ### ### ### ### ### ### ### # [100%]

これでgitはインストール可能になるのですが、このままではepelリポジトリがCentOSの公式リポジトリより優先されてしまうので、公式リポジトリを優先するように設定します。


## yum-prioritiesのインストール



[vagrant@vagrant-c5-x86_64 ~]$ sudo yum install yum-priorities
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.grandcloud.cn
 * epel: ftp.kddilabs.jp
 * extras: ftp.riken.jp
 * updates: mirrors.grandcloud.cn
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package yum-priorities.noarch 0:1.1.16-21.el5.centos set to be updated
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================================================================================================
 Package                                                     Arch                                                Version                                                              Repository                                         Size
==============================================================================================================================================================================================================================================
Installing:
 yum-priorities                                              noarch                                              1.1.16-21.el5.centos                                                 base                                               16 k

Transaction Summary
==============================================================================================================================================================================================================================================
Install       1 Package(s)
Upgrade       0 Package(s)

Total download size: 16 k
Is this ok [y/N]: y
Downloading Packages:
yum-priorities-1.1.16-21.el5.centos.noarch.rpm                                                                                                                                                                         |  16 kB     00:00
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : yum-priorities                                                                                                                                                                                                         1/1
error: failed to stat /vagrant: No such file or directory

Installed:
  yum-priorities.noarch 0:1.1.16-21.el5.centos

Complete!


## 優先度の変更


先ほどインストールしたyum-prioritesによりBaseリポジトリにpriorityで優先度を付ける事ができます。


[vagrant@vagrant-c5-x86_64 ~]$ sudo vim /etc/yum.repos.d/CentOS-Base.repo


[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
+priority=1

#released updates
[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
+priority=1

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
+priority=1

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
+priority=1

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib
#baseurl=http://mirror.centos.org/centos/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
+priority=1


## gitのインストール



[vagrant@vagrant-c5-x86_64 ~]$ sudo yum install git
Loaded plugins: fastestmirror, priorities
Loading mirror speeds from cached hostfile
 * base: ftp.riken.jp
 * epel: ftp.kddilabs.jp
 * extras: ftp.riken.jp
 * updates: ftp.riken.jp
base                                                                                                                                                                                                                   | 1.1 kB     00:00
extras                                                                                                                                                                                                                 | 2.1 kB     00:00
updates                                                                                                                                                                                                                | 1.9 kB     00:00
274 packages excluded due to repository priority protections
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package git.x86_64 0:1.8.2.1-1.el5 set to be updated
--> Processing Dependency: perl-Git = 1.8.2.1-1.el5 for package: git
--> Processing Dependency: rsync for package: git
--> Processing Dependency: perl(Term::ReadKey) for package: git
--> Processing Dependency: perl(Git) for package: git
--> Processing Dependency: perl(Error) for package: git
--> Processing Dependency: libcurl.so.3()(64bit) for package: git
--> Running transaction check
---> Package curl.x86_64 0:7.15.5-17.el5_9 set to be updated
--> Processing Dependency: libidn.so.11()(64bit) for package: curl
---> Package perl-Error.noarch 1:0.17010-1.el5 set to be updated
---> Package perl-Git.x86_64 0:1.8.2.1-1.el5 set to be updated
---> Package perl-TermReadKey.x86_64 0:2.30-4.el5 set to be updated
---> Package rsync.x86_64 0:3.0.6-4.el5_7.1 set to be updated
--> Running transaction check
---> Package libidn.x86_64 0:0.6.5-1.1 set to be updated
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================================================================================================
 Package                                                        Arch                                                 Version                                                         Repository                                          Size
==============================================================================================================================================================================================================================================
Installing:
 git                                                            x86_64                                               1.8.2.1-1.el5                                                   epel                                               7.4 M
Installing for dependencies:
 curl                                                           x86_64                                               7.15.5-17.el5_9                                                 base                                               232 k
 libidn                                                         x86_64                                               0.6.5-1.1                                                       base                                               195 k
 perl-Error                                                     noarch                                               1:0.17010-1.el5                                                 epel                                                26 k
 perl-Git                                                       x86_64                                               1.8.2.1-1.el5                                                   epel                                                49 k
 perl-TermReadKey                                               x86_64                                               2.30-4.el5                                                      epel                                                32 k
 rsync                                                          x86_64                                               3.0.6-4.el5_7.1                                                 base                                               347 k

Transaction Summary
==============================================================================================================================================================================================================================================
Install       7 Package(s)
Upgrade       0 Package(s)

Total download size: 8.2 M
Is this ok [y/N]: y
Downloading Packages:
(1/7): perl-Error-0.17010-1.el5.noarch.rpm                                                                                                                                                                             |  26 kB     00:00
(2/7): perl-TermReadKey-2.30-4.el5.x86_64.rpm                                                                                                                                                                          |  32 kB     00:00
(3/7): perl-Git-1.8.2.1-1.el5.x86_64.rpm                                                                                                                                                                               |  49 kB     00:00
(4/7): libidn-0.6.5-1.1.x86_64.rpm                                                                                                                                                                                     | 195 kB     00:01
(5/7): curl-7.15.5-17.el5_9.x86_64.rpm                                                                                                                                                                                 | 232 kB     00:02
(6/7): rsync-3.0.6-4.el5_7.1.x86_64.rpm                                                                                                                                                                                | 347 kB     00:01
(7/7): git-1.8.2.1-1.el5.x86_64.rpm                                                                                                                                                                                    | 7.4 MB     00:25
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                         249 kB/s | 8.2 MB     00:33
警告: rpmts_HdrFromFdno: ヘッダ V3 DSA signature: NOKEY, key ID 217521f6
epel/gpgkey                                                                                                                                                                                                            | 1.7 kB     00:00
Importing GPG key 0x217521F6 "Fedora EPEL <epel@fedoraproject.org>" from /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL
Is this ok [y/N]: y
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : perl-Error                                                                                                                                                                                                             1/7
error: failed to stat /vagrant: No such file or directory
  Installing     : libidn                                                                                                                                                                                                                 2/7
  Installing     : curl                                                                                                                                                                                                                   3/7
  Installing     : perl-TermReadKey                                                                                                                                                                                                       4/7
  Installing     : rsync                                                                                                                                                                                                                  5/7
  Installing     : git                                                                                                                                                                                                                    6/7
  Installing     : perl-Git                                                                                                                                                                                                               7/7

Installed:
  git.x86_64 0:1.8.2.1-1.el5

Dependency Installed:
  curl.x86_64 0:7.15.5-17.el5_9        libidn.x86_64 0:0.6.5-1.1        perl-Error.noarch 1:0.17010-1.el5        perl-Git.x86_64 0:1.8.2.1-1.el5        perl-TermReadKey.x86_64 0:2.30-4.el5        rsync.x86_64 0:3.0.6-4.el5_7.1

Complete!

gitのインストール完了です。