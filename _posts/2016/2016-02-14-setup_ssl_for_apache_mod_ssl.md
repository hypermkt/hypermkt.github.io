---
layout: post
title: Apache2.2+mod_sslのSSL設定メモ
post_id: 644
categories: 
- Apache
---

## 概要



*  Apache2.2で構築されたウェブサーバーにSSL証明書を設定します


## 購入元


SSL証明書は自分が知るかぎり格安の
[RapidSSL](http://www.rapid-ssl.jp/)から購入しました。当日中に発行できるのでとても便利です。


## 設定方法



### mod_sslのインストール


ApacheでSSLを利用するには
[mod_sslモジュール](http://httpd.apache.org/docs/2.2/en/mod/mod_ssl.html)を使います。下記コマンドを実行しmod_sslをインストールします。


$ yum install mod_ssl


### 証明書の配置


購入元からメールで送られてきた証明書ファイルをサーバー内に配置します。下記は一例ですが、パッケージインストールされたhttpdの場合は 
/etc/httpd以下にプログラム・設定ファイルが配置されるので今回は
/etc/httpd/sslディレクトリ内に証明書ファイル３つを配備しました。


$ ll /etc/httpd/ssl
合計 12
-rw-r--r-- 1 root root 1497  2月 13 15:27 www.your-site.jp-cert-chain.pem  # 中間証明書
-rw-r--r-- 1 root root 1513  2月 11 18:19 www.your-site.jp-cert.pem        # サーバー証明書
-rw-r--r-- 1 root root 1679  2月 11 18:19 www.your-site.jp-key.pem         # 秘密鍵


### Apache設定


mod_sslをインストールすると 
/etc/httpd/conf.d/ssl.confに設定ファイルが配置されるので、ssl.confを下記のように設定すると有効になります。（自分用のメモのため各設定項目の説明やその他設定項目は省いています。）


Listen 443
NameVirtualHost *:443

<VirtualHost *:443>
   SSLEngine on
   # SSLv2, SSLv3以外の全てを有効にする
   SSLProtocol all -SSLv2 -SSLv3
   # SSLハンドシェイク時に利用する暗号化方式はサーバー側を優先する
   SSLHonorCipherOrder On
   # 暗号化スイート設定
   SSLCipherSuite ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!EXP:!LOW:!aNULL:!eNULL:!ADH:!DSS:!MD5:!PSK:!SRP:!RC4:!3DES
   SSLCertificateChainFile /etc/httpd/ssl/www.anikore.jp-cert-chain.pem
   SSLCertificateFile /etc/httpd/ssl/www.your-site.jp.jp-cert.pem
   SSLCertificateKeyFile /etc/httpd/ssl/www.your-site.jp.jp-key.pem


### Apache設定の反映


証明書ファイルを配備、Apache設定をしただけではSSLは有効になりません。下記のようにApacheを再起動して初めて有効になります。


$ sudo service httpd configtest
$ sudo service httpd graceful


## 感想


SSLを有効にすること自体は参考サイト通りにすれば簡単に設定はできる事は分った。但し近年のSSL脆弱性問題に対応するには正しい知識と理解から利用する暗号化スイートは選択しなければならない点は気をつける必要がある。