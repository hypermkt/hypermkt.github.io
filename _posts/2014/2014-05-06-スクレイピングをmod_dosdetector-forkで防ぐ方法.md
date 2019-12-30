---
layout: post
title: スクレイピングをmod_dosdetector-forkで防ぐ方法
post_id: 260
categories: 
- Apache
---

とある日、サーバーがやけに重いなーと思ってログを見た所、頑張ってスクレイピングをしている人がいた。その日のログの件数だけで18万件・・・。111.86.200.225 - - [18/Apr/2014:13:38:24 +0900] "GET /review/698049/ HTTP/1.1" 200 8703 "http://www.anikore.jp/anime_review/4031/page:3" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:28.0) Gecko/20100101 Firefox/28.0"
111.86.200.225 - - [18/Apr/2014:13:38:19 +0900] "GET /anime_review/4031/page:3 HTTP/1.1" 200 15619 "http://www.anikore.jp/anime_review/4031/" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:28.0) Gecko/20100101 Firefox/28.0"
111.86.200.225 - - [18/Apr/2014:13:38:19 +0900] "GET /review/555875/ HTTP/1.1" 200 10559 "http://www.anikore.jp/anime_review/4030/" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:28.0) Gecko/20100101 Firefox/28.0"
111.86.200.225 - - [18/Apr/2014:13:38:20 +0900] "GET /review/715718/ HTTP/1.1" 200 10455 "http://www.anikore.jp/anime_review/4030/" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:28.0) Gecko/20100101 Firefox/28.0"
111.86.200.225 - - [18/Apr/2014:13:38:20 +0900] "GET /review/611081/ HTTP/1.1" 200 9340 "http://www.anikore.jp/anime_review/4031/page:5" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:28.0) Gecko/20100101 Firefox/28.0"

レビューサイトの宿命ではあるんだが、結構スクレイピングをされる事が多いです。どうせやるならバレないようにしてほしいんですが、一気にアクセスされるとさすがにサーバーに負荷がかかり、他のユーザーさんにも影響が出てしまうため、対策を行いました。

スクレイピングもほぼDoS攻撃と連続アクセスという点では同じなので、DoS対策について調べました。


*  mod_security


*  mod_evasive


*  mod_dosdetector


*  mod_dosdetector-fork


## 導入方法



### ビルド


ビルド自体は簡単。下記の通りすれば完了。


git clone git@github.com:tkyk/mod_dosdetector-fork.git
cd /usr/local/src/mod_dosdetector-fork
make
make install


### 導入


ビルドしたモジュールをhttpd.confで読み込む


LoadModule dosdetector_module /usr/lib64/httpd/modules/mod_dosdetector.so

DoS対策をしたバーチャルホスト設定内に下記を追加。ポイントと下記の通りです。


*  ローカルネットワーク内からのアクセスは全て除外する。定期的な監視スクリプトなどが走っているので。


*  クローラーは除外する


*  画像、CSS、Javascriptなどはアクセスカウントの対象外とする


# デフォルトでは favicon のコンテンツタイプは指定されていないので設定
AddType image/vnd.microsoft.icon .ico

# 検知を有効にする
DoSDetection On

# DoS 攻撃の判定を行う時間を設定。（秒）
DoSPeriod 60

# DoSPeriod の間にこの数だけアクセスがあれば DoS 攻撃の疑いありとみなし、環境変数 SuspectDoS を 1 にセットする。
DoSThreshold 50

# DoSPeriod の間にこの数だけアクセスがあれば DoS 攻撃の疑いが強いとみなし、環境変数 SuspectHardDoS を 1 にセットする。
DoSHardThreshold 100

# DoS 攻撃の疑いが設定されてから解除するまでの時間。（秒）
DoSBanPeriod 3600

# クライアントの追跡記録を保存する数。多すぎるとその分リソースを消費する。
DoSTableSize 100


SetEnvIf Request_URI "\.(gif|jpe?g|ico|js|css|png)$" NoCheckDoS

RewriteEngine On
RewriteCond %{ENV:SuspectHardDoS} =1
RewriteCond %｛REMOTE_ADDR｝ !^(127\.0\.0\.1)$
# クラスAのローカルIPアドレス帯を全て除外
RewriteCond %｛REMOTE_ADDR｝ !^(10\.[0-9]+\.[0-9]\.[0-9])$
# クラスBのローカルIPアドレス帯を全て除外
RewriteCond %｛REMOTE_ADDR｝ !^(172\.(1[6-9]|2[0-9]|3[0-1])\.[0-9]+\.[0-9]+)$
# クラスCのローカルIPアドレス帯を全て除外
RewriteCond %｛REMOTE_ADDR｝ !^(192\.168\.[0-9]+\.[0-9]+)$
RewriteCond %｛HTTP_USER_AGENT｝ !(google|yahoo|msn|bing) [NC]
RewriteRule .* - [R=503,L]
ErrorDocument 503 "Server is busy."

LogFormat "%{SuspectHardDoS}e %h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" dos_suspect
CustomLog "logs/dos_suspect_log" dos_suspect env=SuspectDoS

あとはApacheをconfigtest & gracefulすれば完了です。


## 運用後


運用後に気付いた点をまとめます。


### クローラーがひっかかる


下記はbingの一例ですが、Googleクローラーも疑わしいアクセスとして検知されます。


[Tue May 06 04:03:50 2014] [notice] mod_dosdetector: '157.55.33.26' is suspected as DoS attack! (counter: 51)


157.55.33.26 - - [06/May/2014:11:24:03 +0900] "GET /review/255621/ HTTP/1.0" 200 85004 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
157.55.33.26 - - [06/May/2014:11:24:13 +0900] "GET /review/683640/ HTTP/1.0" 200 38173 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"

もちろんクローラーをDoSアタックと検知し、拒否されてしまうとSEO対策的に好ましくないので、上記の設定時に各種クローラーは除外するようにし拒否しないようにします。


RewriteCond %｛HTTP_USER_AGENT｝ !(google|yahoo|msn|bing) [NC]


### 怪しいアクセスを防いだ


ユーザーエージェントもなく連続アクセスっぽいアクセスがあったが、１０１アクセス目で503を返しており、拒否っています。あちら側も503を検知したのか急にアクセスが止まった。


61.119.116.18 - - [06/May/2014:03:04:54 +0900] "GET /tag/%E8%85%90%E5%A5%B3%E5%AD%90%E5%90%91%E3%81%91/ HTTP/1.1" 200 20594 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:55 +0900] "GET /tag/%E3%81%BB%E3%81%AE%E3%81%BC%E3%81%AE/ HTTP/1.1" 200 22122 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:56 +0900] "GET /tag/%E9%80%86%E3%83%8F%E3%83%BC%E3%83%AC%E3%83%A0/ HTTP/1.1" 200 19831 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:57 +0900] "GET /project/ HTTP/1.1" 200 5999 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:57 +0900] "GET /terms_of_use/ HTTP/1.1" 200 9861 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:58 +0900] "GET /guideline/ HTTP/1.1" 503 15 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:58 +0900] "GET /privacy_policy/ HTTP/1.1" 503 15 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:58 +0900] "GET /thank_ranking/ HTTP/1.1" 503 15 "-" "-"
61.119.116.18 - - [06/May/2014:03:04:58 +0900] "GET /contacts/ HTTP/1.1" 503 15 "-" "-"

とまぁまだまだ様子見の段階ではありますが、概ね順調に動いております。これで次回スクレイピングやDoSアタックが来ても大丈夫！でありたい。