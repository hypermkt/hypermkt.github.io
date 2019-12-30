---
layout: post
title: Macでvagrant upの際にエラーが発生した際の対処法
post_id: 53
categories: 
- Mac
- Vagrant
---

[![logo_wide-cab47086](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/logo_wide-cab47086-300x82.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/logo_wide-cab47086.png)

過去に何十回とぐぐったのでそろそろ自分用のメモとしてまとめておく。

vagrant upをすると稀に下記のエラーが表示される


$ vagrant up
Bringing machine 'manage' up with 'virtualbox' provider...
[manage] Clearing any previously set forwarded ports...
[manage] Creating shared folders metadata...
[manage] Clearing any previously set network interfaces...
There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.

Command: ["hostonlyif", "create"]

Stderr: 0%...
Progress state: NS_ERROR_FAILURE
VBoxManage: error: Failed to create the host-only adapter
VBoxManage: error: VBoxNetAdpCtl: Error while adding new interface: failed to open /dev/vboxnetctl: No such file or directory

VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component HostNetworkInterface, interface IHostNetworkInterface
VBoxManage: error: Context: "int handleCreate(HandlerArg*, int, int*)" at line 68 of file VBoxManageHostonly.cpp

この場合は下記コマンドを実行してVirtual Boxを再起動したら直ります。なぜ発生するかの原因は不明ですが・・・。


$ sudo /Library/StartupItems/VirtualBox/VirtualBox restart