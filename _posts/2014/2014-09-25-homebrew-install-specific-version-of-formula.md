---
layout: post
title: homebrewパッケージを指定バージョンでインストールする
post_id: 417
categories: 
- homebrew
- Mac
- MySQL
---

３回ぐぐったらまとめようルールにひかかったのでまとめます。

諸事情でMacの中に入っているmysqlのバージョンを落とす必要があったのでバージョンを下げる方法をまとめます。##まずインストール可能なバージョンを調べる



brew versions #{package_name}でバージョンを調べます。ここでは
5.5.29をインストールしたいとします。


$ brew versions mysql
Warning: brew-versions is unsupported and may be removed soon.
Please use the homebrew-versions tap instead:
  https://github.com/Homebrew/homebrew-versions
5.6.19   git checkout 3d3d8ae /usr/local/Library/Formula/mysql.rb
5.6.17   git checkout e271c21 /usr/local/Library/Formula/mysql.rb
5.6.16   git checkout 34ad88d /usr/local/Library/Formula/mysql.rb
5.6.15   git checkout b8a121e /usr/local/Library/Formula/mysql.rb
5.6.14   git checkout dc5d10c /usr/local/Library/Formula/mysql.rb
5.6.13   git checkout dcc7f40 /usr/local/Library/Formula/mysql.rb
5.6.12   git checkout ed829a3 /usr/local/Library/Formula/mysql.rb
5.6.10   git checkout 48f7e86 /usr/local/Library/Formula/mysql.rb
5.5.29   git checkout 336c976 /usr/local/Library/Formula/mysql.rb
5.5.28   git checkout 5825f62 /usr/local/Library/Formula/mysql.rb
5.5.27   git checkout 93aecfa /usr/local/Library/Formula/mysql.rb
5.5.25a  git checkout faaa6c1 /usr/local/Library/Formula/mysql.rb
5.5.25   git checkout 5bcd1f3 /usr/local/Library/Formula/mysql.rb
5.5.24   git checkout a977fbd /usr/local/Library/Formula/mysql.rb
5.5.20   git checkout f4009ef /usr/local/Library/Formula/mysql.rb
5.5.19   git checkout c2b8e87 /usr/local/Library/Formula/mysql.rb
5.5.15   git checkout 513c0f8 /usr/local/Library/Formula/mysql.rb
5.5.14   git checkout a840c4a /usr/local/Library/Formula/mysql.rb
5.5.12   git checkout 35b60ca /usr/local/Library/Formula/mysql.rb
5.5.10   git checkout 1f2b24e /usr/local/Library/Formula/mysql.rb
5.1.56   git checkout feae63a /usr/local/Library/Formula/mysql.rb
5.1.55   git checkout 0476235 /usr/local/Library/Formula/mysql.rb
5.1.54   git checkout 7077f84 /usr/local/Library/Formula/mysql.rb
5.1.53   git checkout fbee3a0 /usr/local/Library/Formula/mysql.rb
5.1.52   git checkout 7871a99 /usr/local/Library/Formula/mysql.rb
5.1.51   git checkout 0fc5ce3 /usr/local/Library/Formula/mysql.rb
5.1.49   git checkout 2e7d624 /usr/local/Library/Formula/mysql.rb
5.1.48   git checkout 75def21 /usr/local/Library/Formula/mysql.rb
5.1.47   git checkout 9d73887 /usr/local/Library/Formula/mysql.rb
5.1.46   git checkout ee9ecec /usr/local/Library/Formula/mysql.rb
5.1.45   git checkout 1ca6b6b /usr/local/Library/Formula/mysql.rb
5.1.44   git checkout 052d1f2 /usr/local/Library/Formula/mysql.rb
5.1.43   git checkout c4decd7 /usr/local/Library/Formula/mysql.rb


## 対象Formulaファイル？のバージョンを下げます



git checkout 336c976 /usr/local/Library/Formula/mysql.rb


## あとはインストールするだけです


```sh
$ brew install mysql
==> Installing mysql dependency: cmake
==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/cmake-3.0.0.mavericks.bottle.tar.gz
### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ### ###  100.0%
==> Pouring cmake-3.0.0.mavericks.bottle.tar.gz