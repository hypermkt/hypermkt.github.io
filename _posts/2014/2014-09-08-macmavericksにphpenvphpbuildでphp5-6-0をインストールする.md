---
layout: post
title: Mac(Mavericks)にphpenv+phpbuildでPHP5.6.0をインストールする
post_id: 381
categories: 
- 未分類
---

先日ついにPHP5.6.0が正式リリースし、同僚からも「ちゃんとPHP5.6.0をキャッチアップしてる？？」と煽られたので早速ローカル環境にPHP5.6.0をインストールします。##準備



### phpenvをインストール



cd
git clone git://github.com/phpenv/phpenv.git .phpenv
echo 'export PATH="$HOME/.phpenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(phpenv init -)"' >> ~/.bash_profile


### php-buildをインストール



git clone git://github.com/CHH/php-build.git $HOME/.phpenv/plugins/php-build


## PHP5.6.0のインストール



$ php-build 5.6.0 ~/.phpenv/versions/5.6.0
[Info]: Loaded apc Plugin.
[Info]: Loaded pyrus Plugin.
[Info]: Loaded xdebug Plugin.
[Info]: Loaded xhprof Plugin.
[Info]: php.ini-production gets used as php.ini
[Info]: Building 5.6.0 into /Users/#{username}/.phpenv/versions/5.6.0
[Downloading]: http://php.net/distributions/php-5.6.0.tar.bz2
[Preparing]: /var/tmp/php-build/source/5.6.0
[Compiling]: /var/tmp/php-build/source/5.6.0
[Pyrus]: Downloading from http://pear2.php.net/pyrus.phar
[Pyrus]: Installing executable in /Users/#{username}/.phpenv/versions/5.6.0/bin/pyrus
[XDebug]: Downloading http://xdebug.org/files/xdebug-2.2.5.tgz
[XDebug]: Compiling in /var/tmp/php-build/source/xdebug-2.2.5
[XDebug]: Installing XDebug configuration in /Users/#{username}/.phpenv/versions/5.6.0/etc/conf.d/xdebug.ini
[XDebug]: Cleaning up.
[Info]: Enabling Opcache...
[Info]: Done
[Info]: The Log File is not empty, but the Build did not fail. Maybe just warnings got logged. You can review the log in /tmp/php-build.5.6.0.20140907105041.log
[Success]: Built 5.6.0 successfully.


## PHP5.6.0に切り替える



*   system (set by /Users/#{username}/.phpenv/version)
  5.6.0
$ phpenv local 5.6.0
phpenv v0.0.4-dev

5.6.0
$ phpenv rehash
$ php -v
PHP 5.6.0 (cli) (built: Sep  7 2014 10:59:24) 
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2014 Zend Technologies
    with Zend OPcache v7.0.4-dev, Copyright (c) 1999-2014, by Zend Technologies
    with Xdebug v2.2.5, Copyright (c) 2002-2014, by Derick Rethans