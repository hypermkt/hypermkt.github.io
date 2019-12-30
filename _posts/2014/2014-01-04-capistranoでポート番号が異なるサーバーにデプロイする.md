---
layout: post
title: Capistranoでポート番号が異なるサーバーにデプロイする方法
post_id: 90
categories: 
- Capistrano
---

[![capistrano1](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/capistrano1-e1388846470181-300x89.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/capistrano1-e1388846470181.png)


## 前提



*  Capistranoのssh_optionsのport設定は使用せず、基本的には22番ポートの接続を前提とします。


*  Capistranoのroleには22番ポートのサーバー、それ以外のポートのサーバーもまとめて記述。


## 1. ~/.ssh/configに22番ポート以外の接続先別のポート番号指定を記述する



Host xxx.xxx.xxx.xxx
  HostName xxx.xxx.xxx.xxx
  Port 10022


## 2. リモート先の~/.ssh/configにGitHubへの接続ポートを指定する


実際のエラーをメモし忘れましたが、１番の対応のままデプロイをすると「GitHubに10022ポートで接続できないよ」というエラーが発生しデプロイに失敗します。それを正す為に下記でGitHubへの接続を22番ポートに強制します。


Host github.com
  HostName github.com
  Port 22
  User git

これで複数台のサーバーの一部のポート番号が異なる場合でもCapistranoでデプロイ可能となります。
が、そもそもこのような例外対応をしなくてはいけない状況自体が問題なので、sshの接続ポートをまずは揃えた方が良いと思います。