---
layout: post
title: CentOS5にPercona Tool Kitをインストールする方法
post_id: 809
categories: 
- Centos
- MySQL
---

約5,000万件のテーブルにインデックスを張らなくてはいけなくなりました。明らかに時間がかかることが明白でしたので、Percona Tool Kitの pt-online-schema-change を使うことにしました。これを使うことで安全かつ高速にインデックス追加ができるようになります。今回はCentOS5にPercona Tool Kitをインストールする方法をまとめます。(なんで今さらCentOS5なの？ということはスルーして下さい・・・)##pt-olnine-schema-change とは



[Percona社](https://www.percona.com/) が提供しているMySQL用の便利ツール集 
[Percona Tool Kit](https://www.percona.com/software/mysql-tools/percona-toolkit)の一つです。pt-online-schema-changeを利用することでサービスを無停止でカラム、インデックス追加等の操作が実行できます。詳しくは
[こちら](http://d.hatena.ne.jp/fat47/20140418/1397811745) をご覧ください。図付きの解説が分かりやすくまとまっています。

 


## インストール方法


英語ですが手順は
[本家のサイト](https://www.percona.com/doc/percona-server/5.5/installation/yum_repo.html)にまとまっています。それを参考にインストールを進めます。


### Percona Yum Repositoryのインストール


まずはpercona-release-0.1-3.noarch.rpmをダウンロードします。CentOS5ではリモートのyumを直接インストールすることが出来ないのでwgetする必要があります。


[root@www ~]# wget http://www.percona.com/downloads/percona-release/redhat/0.1-3/percona-release-0.1-3.noarch.rpm
--2016-08-20 18:58:37-- http://www.percona.com/downloads/percona-release/redhat/0.1-3/percona-release-0.1-3.noarch.rpm
Resolving www.percona.com... 74.121.199.234
Connecting to www.percona.com|74.121.199.234|:80... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://www.percona.com/downloads/percona-release/redhat/0.1-3/percona-release-0.1-3.noarch.rpm [following]
--2016-08-20 18:58:38-- https://www.percona.com/downloads/percona-release/redhat/0.1-3/percona-release-0.1-3.noarch.rpm
Connecting to www.percona.com|74.121.199.234|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 6566 (6.4K) [application/x-redhat-package-manager]
Saving to: `percona-release-0.1-3.noarch.rpm'

100%[===========================================================================================================================================&gt;] 6,566 --.-K/s in 0s

2016-08-20 18:58:39 (88.2 MB/s) - `percona-release-0.1-3.noarch.rpm' saved [6566/6566]

ダウンロードしたらrpmコマンドでインストールします。


[root@www ~]# ll
total 56
-rw------- 1 root root 1126 Aug 31 2012 anaconda-ks.cfg
-rw-r--r-- 1 root root 23614 Aug 31 2012 install.log
-rw-r--r-- 1 root root 3536 Aug 31 2012 install.log.syslog
-rw-rw-r-- 1 root root 6566 Sep 22 2014 percona-release-0.1-3.noarch.rpm
[root@www ~]# rpm -ivh percona-release-0.1-3.noarch.rpm
warning: percona-release-0.1-3.noarch.rpm: Header V4 DSA signature: NOKEY, key ID cd2efd2a
Preparing... ### ### ### ### ### ### ### ### ### ### ### ### ### ### # [100%]
1:percona-release ### ### ### ### ### ### ### ### ### ### ### ### ### ### # [100%]

これで完了。


### Perlcona Tool Kitのインストール


このようにインストール可能な状態となります。


[root@www ~]# yum list | grep percona
percona-release.noarch 0.1-3 installed
Percona-SQL-50-debuginfo.x86_64 5.0.92-b23.89.rhel5 percona-release-x86_64
Percona-SQL-client-50.x86_64 5.0.92-b23.89.rhel5 percona-release-x86_64
Percona-SQL-devel-50.x86_64 5.0.92-b23.89.rhel5 percona-release-x86_64
Percona-SQL-server-50.x86_64 5.0.92-b23.89.rhel5 percona-release-x86_64
Percona-SQL-shared-50.x86_64 5.0.92-b23.89.rhel5 percona-release-x86_64
Percona-SQL-shared-compat.x86_64 5.0.92-b23.85.rhel5 percona-release-x86_64
Percona-SQL-test-50.x86_64 5.0.92-b23.89.rhel5 percona-release-x86_64
Percona-Server-51-debuginfo.x86_64 5.1.73-rel14.12.624.rhel5 percona-release-x86_64
Percona-Server-55-debuginfo.x86_64 5.5.51-rel38.1.el5 percona-release-x86_64
Percona-Server-56-debuginfo.x86_64 5.6.31-rel77.0.el5 percona-release-x86_64
Percona-Server-MongoDB.x86_64 3.0.12-1.8.el5 percona-release-x86_64
Percona-Server-MongoDB-32.x86_64 3.2.8-2.0.el5 percona-release-x86_64
3.2.8-2.0.el5 percona-release-x86_64
Percona-Server-MongoDB-32-mongos.x86_64 3.2.8-2.0.el5 percona-release-x86_64
Percona-Server-MongoDB-32-server.x86_64 3.2.8-2.0.el5 percona-release-x86_64
Percona-Server-MongoDB-32-shell.x86_64 3.2.8-2.0.el5 percona-release-x86_64
Percona-Server-MongoDB-32-tools.x86_64 3.2.8-2.0.el5 percona-release-x86_64
Percona-Server-MongoDB-debuginfo.x86_64 3.0.12-1.8.el5 percona-release-x86_64
Percona-Server-MongoDB-mongos.x86_64 3.0.12-1.8.el5 percona-release-x86_64
Percona-Server-MongoDB-server.x86_64 3.0.12-1.8.el5 percona-release-x86_64
Percona-Server-MongoDB-shell.x86_64 3.0.12-1.8.el5 percona-release-x86_64
Percona-Server-MongoDB-tools.x86_64 3.0.12-1.8.el5 percona-release-x86_64
Percona-Server-client-51.x86_64 5.1.73-rel14.12.624.rhel5 percona-release-x86_64
Percona-Server-client-55.x86_64 5.5.51-rel38.1.el5 percona-release-x86_64
Percona-Server-client-56.x86_64 5.6.31-rel77.0.el5 percona-release-x86_64
Percona-Server-devel-51.x86_64 5.1.73-rel14.12.624.rhel5 percona-release-x86_64
Percona-Server-devel-55.x86_64 5.5.51-rel38.1.el5 percona-release-x86_64
Percona-Server-devel-56.x86_64 5.6.31-rel77.0.el5 percona-release-x86_64
Percona-Server-server-51.x86_64 5.1.73-rel14.12.624.rhel5 percona-release-x86_64
Percona-Server-server-55.x86_64 5.5.51-rel38.1.el5 percona-release-x86_64
Percona-Server-server-56.x86_64 5.6.31-rel77.0.el5 percona-release-x86_64
Percona-Server-shared-51.x86_64 5.1.73-rel14.12.624.rhel5 percona-release-x86_64
Percona-Server-shared-55.x86_64 5.5.51-rel38.1.el5 percona-release-x86_64
Percona-Server-shared-56.x86_64 5.6.31-rel77.0.el5 percona-release-x86_64
Percona-Server-shared-compat.x86_64 5.1.68-rel14.6.551.rhel5 percona-release-x86_64
Percona-Server-shared-compat-51.x86_64 5.1.73-rel14.12.624.rhel5 percona-release-x86_64
Percona-Server-test-51.x86_64 5.1.73-rel14.12.624.rhel5 percona-release-x86_64
Percona-Server-test-55.x86_64 5.5.51-rel38.1.el5 percona-release-x86_64
Percona-Server-test-56.x86_64 5.6.31-rel77.0.el5 percona-release-x86_64
Percona-Server-tokudb-56.x86_64 5.6.31-rel77.0.el5 percona-release-x86_64
Percona-XtraDB-Cluster-55.x86_64 1:5.5.41-25.11.853.el5 percona-release-x86_64
1:5.5.41-25.11.853.el5 percona-release-x86_64
Percona-XtraDB-Cluster-56.x86_64 1:5.6.30-25.16.1.el5 percona-release-x86_64
1:5.6.30-25.16.1.el5 percona-release-x86_64
Percona-XtraDB-Cluster-client.x86_64 1:5.5.34-23.7.6.565.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-client-55.x86_64 1:5.5.41-25.11.853.el5 percona-release-x86_64
Percona-XtraDB-Cluster-client-56.x86_64 1:5.6.30-25.16.1.el5 percona-release-x86_64
Percona-XtraDB-Cluster-debuginfo.x86_64 1:5.5.34-23.7.6.565.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-devel.x86_64 1:5.5.34-23.7.6.565.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-devel-55.x86_64 1:5.5.41-25.11.853.el5 percona-release-x86_64
Percona-XtraDB-Cluster-devel-56.x86_64 1:5.6.30-25.16.1.el5 percona-release-x86_64
Percona-XtraDB-Cluster-full-55.x86_64 1:5.5.41-25.11.853.el5 percona-release-x86_64
Percona-XtraDB-Cluster-full-56.x86_64 1:5.6.30-25.16.1.el5 percona-release-x86_64
Percona-XtraDB-Cluster-galera.x86_64 2.8-1.162.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-galera-2.x86_64 2.12-1.2682.rhel5 percona-release-x86_64
2.12-1.2682.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-galera-3.x86_64 3.16-1.rhel5 percona-release-x86_64
3.16-1.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-galera-56.x86_64 3.1-1.169.rhel5 percona-release-x86_64
3.1-1.169.rhel5 percona-release-x86_64
2.8-1.162.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-garbd-2.x86_64 2.12-1.2682.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-garbd-3.x86_64 3.16-1.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-server.x86_64 1:5.5.34-23.7.6.565.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-server-55.x86_64 1:5.5.41-25.11.853.el5 percona-release-x86_64
Percona-XtraDB-Cluster-server-56.x86_64 1:5.6.30-25.16.1.el5 percona-release-x86_64
Percona-XtraDB-Cluster-shared.x86_64 1:5.5.34-23.7.6.565.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-shared-55.x86_64 1:5.5.41-25.11.853.el5 percona-release-x86_64
Percona-XtraDB-Cluster-shared-56.x86_64 1:5.6.30-25.16.1.el5 percona-release-x86_64
Percona-XtraDB-Cluster-test.x86_64 1:5.5.34-23.7.6.565.rhel5 percona-release-x86_64
Percona-XtraDB-Cluster-test-55.x86_64 1:5.5.41-25.11.853.el5 percona-release-x86_64
Percona-XtraDB-Cluster-test-56.x86_64 1:5.6.30-25.16.1.el5 percona-release-x86_64
jemalloc.x86_64 3.6.0-2.el5 percona-release-x86_64
jemalloc-devel.x86_64 3.6.0-2.el5 percona-release-x86_64
libev.x86_64 4.15-1.el5.rf percona-release-x86_64
libev-devel.x86_64 4.15-1.el5.rf percona-release-x86_64
libtokumx-enterprise.x86_64 2.0.2-1.el5 percona-release-x86_64
libtokumx-enterprise-devel.x86_64 2.0.2-1.el5 percona-release-x86_64
percona-cacti-templates.noarch 1.1.6-1 percona-release-noarch
percona-nagios-plugins.noarch 1.1.6-1 percona-release-noarch
percona-playback.x86_64 0.7-2.el5.centos percona-release-x86_64
percona-playback-debuginfo.x86_64 0.7-2.el5.centos percona-release-x86_64
percona-playback-devel.x86_64 0.7-2.el5.centos percona-release-x86_64
percona-toolkit.noarch 2.2.19-1 percona-release-noarch
percona-xtrabackup.x86_64 2.3.5-1.el5 percona-release-x86_64
percona-xtrabackup-20.x86_64 2.0.8-587.rhel5 percona-release-x86_64
percona-xtrabackup-20-debuginfo.x86_64 2.0.8-587.rhel5 percona-release-x86_64
percona-xtrabackup-20-test.x86_64 2.0.8-587.rhel5 percona-release-x86_64
percona-xtrabackup-21.x86_64 2.1.9-746.rhel5 percona-release-x86_64
percona-xtrabackup-21-debuginfo.x86_64 2.1.9-746.rhel5 percona-release-x86_64
percona-xtrabackup-22.x86_64 2.2.13-1.el5 percona-release-x86_64
percona-xtrabackup-22-debuginfo.x86_64 2.2.13-1.el5 percona-release-x86_64
percona-xtrabackup-24.x86_64 2.4.4-1.el5 percona-release-x86_64
percona-xtrabackup-24-debuginfo.x86_64 2.4.4-1.el5 percona-release-x86_64
percona-xtrabackup-debuginfo.x86_64 2.3.5-1.el5 percona-release-x86_64
percona-xtrabackup-test.x86_64 2.3.5-1.el5 percona-release-x86_64
percona-xtrabackup-test-21.x86_64 2.1.9-746.rhel5 percona-release-x86_64
percona-xtrabackup-test-22.x86_64 2.2.13-1.el5 percona-release-x86_64
percona-xtrabackup-test-24.x86_64 2.4.4-1.el5 percona-release-x86_64
percona-zabbix-templates.noarch 1.1.6-1 percona-release-noarch
qpress.x86_64 11-1.el5.centos percona-release-x86_64
qpress-debuginfo.x86_64 11-1.el5.centos percona-release-x86_64
sysbench.x86_64 0.5-6.el5 percona-release-x86_64
sysbench-debuginfo.x86_64 0.5-6.el5 percona-release-x86_64
tokumx-enterprise.x86_64 2.0.2-1.el5 percona-release-x86_64
tokumx-enterprise-common.x86_64 2.0.2-1.el5 percona-release-x86_64
tokumx-enterprise-server.x86_64 2.0.2-1.el5 percona-release-x86_64

必要なのは Percona Tool Kit だけなので、それだけをインストールします。


[root@www ~]# yum install percona-toolkit.noarch
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
*   base: mirror.fairway.ne.jp
*   extras: mirror.nus.edu.sg
*   rpmforge: mirror.oscc.org.my
*   updates: centos.usonyx.net
Setting up Install Process
Resolving Dependencies
--&gt; Running transaction check
---&gt; Package percona-toolkit.noarch 0:2.2.19-1 set to be updated
--&gt; Processing Dependency: perl(DBD::mysql) &gt;= 1.0 for package: percona-toolkit
--&gt; Processing Dependency: perl(Term::ReadKey) for package: percona-toolkit
--&gt; Processing Dependency: perl(IO::Socket::SSL) for package: percona-toolkit
--&gt; Running transaction check
---&gt; Package perl-DBD-MySQL.x86_64 0:3.0007-2.el5 set to be updated
--&gt; Processing Dependency: libmysqlclient.so.15(libmysqlclient_15)(64bit) for package: perl-DBD-MySQL
--&gt; Processing Dependency: libmysqlclient.so.15()(64bit) for package: perl-DBD-MySQL
---&gt; Package perl-IO-Socket-SSL.noarch 0:1.01-2.el5 set to be updated
--&gt; Processing Dependency: perl(Net::SSLeay) &gt;= 1.21 for package: perl-IO-Socket-SSL
---&gt; Package perl-TermReadKey.x86_64 0:2.30-3.el5.rf set to be updated
--&gt; Running transaction check
---&gt; Package mysql.x86_64 0:5.0.95-5.el5_9 set to be updated
---&gt; Package perl-Net-SSLeay.x86_64 0:1.30-4.fc6 set to be updated
--&gt; Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================================================================================
Package Arch Version Repository Size
=====================================================================================================================================================================================
Installing:
percona-toolkit noarch 2.2.19-1 percona-release-noarch 1.7 M
Installing for dependencies:
mysql x86_64 5.0.95-5.el5_9 base 4.9 M
perl-DBD-MySQL x86_64 3.0007-2.el5 base 148 k
perl-IO-Socket-SSL noarch 1.01-2.el5 base 50 k
perl-Net-SSLeay x86_64 1.30-4.fc6 base 192 k
perl-TermReadKey x86_64 2.30-3.el5.rf rpmforge 57 k

Transaction Summary
=====================================================================================================================================================================================
Install 6 Package(s)
Upgrade 0 Package(s)

Total download size: 7.0 M
Is this ok [y/N]: y
Downloading Packages:
(1/6): perl-IO-Socket-SSL-1.01-2.el5.noarch.rpm | 50 kB 00:00
(2/6): perl-TermReadKey-2.30-3.el5.rf.x86_64.rpm | 57 kB 00:00
(3/6): perl-DBD-MySQL-3.0007-2.el5.x86_64.rpm | 148 kB 00:00
(4/6): perl-Net-SSLeay-1.30-4.fc6.x86_64.rpm | 192 kB 00:00
(5/6): percona-toolkit-2.2.19-1.noarch.rpm | 1.7 MB 00:01
(6/6): mysql-5.0.95-5.el5_9.x86_64.rpm | 4.9 MB 00:28
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total 209 kB/s | 7.0 MB 00:34
warning: rpmts_HdrFromFdno: Header V4 DSA signature: NOKEY, key ID cd2efd2a
percona-release-noarch/gpgkey | 1.7 kB 00:00
Importing GPG key 0xCD2EFD2A "Percona MySQL Development Team &lt;mysql-dev@percona.com&gt;" from /etc/pki/rpm-gpg/RPM-GPG-KEY-Percona
Is this ok [y/N]: y
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
Installing : perl-TermReadKey 1/6
Installing : perl-Net-SSLeay 2/6
Installing : mysql 3/6
Installing : perl-DBD-MySQL 4/6
Installing : perl-IO-Socket-SSL 5/6
Installing : percona-toolkit 6/6

Installed:
percona-toolkit.noarch 0:2.2.19-1

Dependency Installed:
mysql.x86_64 0:5.0.95-5.el5_9 perl-DBD-MySQL.x86_64 0:3.0007-2.el5 perl-IO-Socket-SSL.noarch 0:1.01-2.el5 perl-Net-SSLeay.x86_64 0:1.30-4.fc6
perl-TermReadKey.x86_64 0:2.30-3.el5.rf

Complete!

これでインストール完了です。コマンドを叩くと使えるようになっているのが分かります。


[root@www ~]# pt-online-schema-change
Usage: pt-online-schema-change [OPTIONS] DSN

Errors in command-line arguments:
*   A DSN must be specified
*   The DSN must specify a database (D) and a table (t)

pt-online-schema-change alters a table's structure without blocking reads or
writes. Specify the database and table in the DSN. Do not use this tool before
reading its documentation and checking your backups carefully. For more
details, please use the --help option, or try 'perldoc
/usr/bin/pt-online-schema-change' for complete documentation.

実際 pt-online-schema-changeしか使ったこと無いので他のコマンドを試してみたいですね。