---
layout: post
title: Mac(mavericks)にcpanmをインストールする
post_id: 356
categories: 
- Perl
---

YAPC 2014 Tokyoに参加してPerl熱があがったので、cpanmコマンドが使いたくなったので、早速インストールしてみた*[Downloading the standalone executable](http://search.cpan.org/~miyagawa/App-cpanminus-1.7004/lib/App/cpanminus.pm#Downloading_the_standalone_executable)

上記サイトを参考にインストールしてみる


$ cd /usr/bin
$ sudo curl -LOk http://xrl.us/cpanm
Password:
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   216  100   216    0     0    367      0 --:--:-- --:--:-- --:--:--   367
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
  0     0    0   131    0     0     67      0 --:--:--  0:00:01 --:--:--   209
100  262k  100  262k    0     0   100k      0  0:00:02  0:00:02 --:--:--  766k
$ ll | grep cpanm
-rw-r--r--   1 root   wheel    268417  8 30 14:10 cpanm
$ sudo chmod +x cpanm
$ cpanm
Usage: cpanm [options] Module [...]

Try `cpanm --help` or `man cpanm` for more options.

と簡単にインストール完了！