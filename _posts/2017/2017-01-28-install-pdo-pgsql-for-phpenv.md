---
layout: post
title: phpenvのphpにpdo_pgsqlをインストールする方法
post_id: 945
categories: 
- PHP
---

## 背景


LaravelからPostgreSQLのデータベースに接続しようと思い、DB設定をした所下記エラーに遭遇した。


Illuminate\Database\QueryException with message 'could not find driver (SQL: select * from "hoges")'

調査した所、下記２点が分かった。


*  Laravelでは
[PDO PostgreSQL 拡張モジュール](http://php.net/manual/ja/ref.pdo-pgsql.php)を利用してPostgreSQLに接続する


*  phpenvでインストールしたPHP7.1.0にはpdo_pgsqlが無い

根本的には２番が原因なので、phpenvでPHPをインストール時にpdo_pgsqlを有効にしてインストールする方法をまとめます。


## 対象者



*  LaravelでPostgreSQLに接続したい人


*  phpenvでpdo_pgsqlを利用したい人


## 手順



### 特定のバージョンで有効にする


7.1.0で有効にする場合は 
~/.phpenv/plugins/php-build/share/php-build/definitions/7.1.0 の冒頭部に 
configure_option -R "--with-pdo-pgsql"を追加するのみです。完成形は下記の通り。


configure_option -R "--with-pdo-pgsql"

install_package "https://secure.php.net/distributions/php-7.1.0.tar.bz2"
install_xdebug "2.5.0"
enable_builtin_opcache


### 全バージョンで有効にする



~/.phpenv/plugins/php-build/share/php-build/default_configure_options の好きなところに 
--with-pdo-pgsqlを追加するのみです。

上記の対応を実施後 
phpenv install 7.1.0 でインストールしなおせば 
pdo_pgsql が有効なPHPがインストールされます。


## 終わりに


phpenvはextensionの設定も細かく調整できて便利でいいですね。