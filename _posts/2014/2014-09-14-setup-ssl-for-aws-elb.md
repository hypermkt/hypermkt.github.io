---
layout: post
title: AWS ELBにSSL証明書を設定する方法
post_id: 398
categories: 
- AWS
- elb
---

AWSのELB&EC2を利用してサイトを運用している場合に、SSLを設定する方法ですが調べた所、ELB側に対して証明書をアップロードする事により対応できるとの事でしたので対応方法をまとめました。##前提



*  Rapid SSLで証明書を購入済みの状態とする


## 設定方法



*  EC2 => Load Balancers => Listnersをクリックする

![step1](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step1.png)


*  EditボタンをクリックするとListners変更画面が表示される

[![step2](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step2.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step2.png)


*  Load Balancer ProtocolのプルダウンよりHTTPS(Secure HTTP)を選択する

![step3](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step3.png)


*  SSL CertificateのChangeボタンを押すとSSLのアップロード画面が表示される。*Private Keyの項目にCSR作成ツールで作成した秘密鍵を貼付ける


*  Public Key Certificateに
「【通知】 SSL サーバ証明書発行完了のお知らせ」メールの
「SSLサーバ証明書（X.509形式）」の箇所を貼付ける


*  Certificate Chainに
「【通知】 SSL サーバ証明書発行完了のお知らせ」メールの
中間証明書(INTERMEDIATE CA):を貼付ける


*  SAVE（ボタン名を忘れました）をクリックして完了

[![step4](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step4.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step4.png)


*  下記の完了画面が表示されたら設定完了です。

[![step5](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step5.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/09/step5.png)


*  SSLのFQDNをhttpsで表示すると正常に動作しているはずです。思っていた以上に簡単で拍子抜けでした。