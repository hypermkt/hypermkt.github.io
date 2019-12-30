---
layout: post
title: ssh port forwardingの使い方
post_id: 793
categories: 
- ssh
---

## ssh port forwarding (ポートフォワーディング)とは



![ssh tunnel](https://hypermkt-blog.lolipop.io/wp-content/uploads/2016/07/ssh-tunnel.png)

別名sshトンネルとも呼ばれており、sshによって確立された通信経路を利用して、クライアントの3307ポートとリモートの3306ポートをマッピングします。

つまりこのポートフォワーディングが接続されている間は3307番に接続すると、リモートサーバーの3306番に接続されます。

しかもssh接続なのでその間の通信は暗号化もされています。


## ssh port fowardingを実行する



ssh -fNg 3307:localhost:3307 user@remote-server -p 22

ローカル環境から上記コマンドを実行することでssh port forwardingが設定できます。


オプション
  
説明

-f
  
バックグラウンドで実行させる

-N
  
リモートコマンドを実行しない。sshポートフォワーディング時のみに利用する

-g
  
リモートホストが転送されたローカルなポートに接続することを許可します。

-L
  
指定されたローカルのポートをリモートのポートとマッピングさせる

 