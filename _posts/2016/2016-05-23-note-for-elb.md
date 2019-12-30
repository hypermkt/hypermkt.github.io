---
layout: post
title: ELB導入時に躓いた件
post_id: 698
categories: 
- AWS
- elb
---

先日ELBを導入したんですが、事前の調査不足もあり思った以上に影響がでてしまい障害となってしまった（反省）。というのも、ELBというAWSのロードバランサー配下にWebサーバーを入れた場合に、クライアントのIPアドレス取得方法が今までと変わる。*[Elastic Load Balancing の X-Forwarded ヘッダー - Elastic Load Balancing](http://docs.aws.amazon.com/ja_jp/ElasticLoadBalancing/latest/DeveloperGuide/x-forwarded-headers.html)


*  [X-Forwarded-For - Wikipedia](https://ja.wikipedia.org/wiki/X-Forwarded-For)

にあるように「X-Forwarded-Forヘッダー」にクライアントIPアドレスが入るようになり 環境変数の
REMOTE_ADDR では取得できないようになる。（知らなかった〜）

これが地味に影響範囲が広く


*  フレームワーク内のIPアドレス取得メソッド


*  Apacheのconfファイル内のIPアドレス制限


*  .htaccess内のIPアドレス制限

に影響があり、アプリケーション全体で盛大に誤動作してしまった・・・。


### フレームワーク内のIPアドレス取得メソッド


PHPであれば 
$_SERVER['HTTP_X_FORWARDED_FOR'];で取得できる。但し複数IPアドレスが入る場合もあるので要注意。
refs : 
[PHP - PHP ロードバランサ経由のクライアントのIP取得ができない(131)｜teratail](https://teratail.com/questions/131)


### .htaccess内のIPアドレス制限


海外のボットなど気軽にIPアドレス制限をかけたい場合に、.htaccessは大変便利なわけなのですが、記述の変更が必要です。
SetEnvIfでIPアドレスを指定する際は正規表現で指定できるのですが、複数個のIPアドレスをパイプでつなげると可読性が悪いので、個人的には下記の記法が好みです。


### ELB非利用時



order allow,deny
allow from all
deny from xxx.xxx.xxx.xxx
deny from xxx.xxx.xxx.xxx


ELB利用時


order allow,deny
allow from all

SetEnvIf X-Forwarded-For "xxx\.xxx\.xxx\.xxx" deny_ip_addresses
SetEnvIf X-Forwarded-For "xxx\.xxx\.xxx\.xxx" deny_ip_addresses

deny from env=deny_ip_addresses

とネット上ではありふれた情報であるのですが、最初は結構戸惑ったので自分用のメモとしてまとめました。どうにか導入できてよかった。これでグーグルクローラーに本気で来られても大丈夫だろう。