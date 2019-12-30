---
layout: post
title: 素のEl CapitanにPHP 5.6.0をインストールする方法
post_id: 817
categories: 
- homebrew
- Mac
- PHP
---

素のMac OSX El CapitanにPHP 5.6.0をインストール機会があったので手順をまとめます。予想通りいろいろつまづきまして・・・。##準備



### Xcode Command Line tool



*  gitコマンドを利用するために必要です


$ xcode-select --install


### Brew



*  PHPビルド時に必要な各種パッケージをインストールするために必要です


$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


### phpenv, php-build


PHPのビルド＆インストールに必要です


$ git clone https://github.com/CHH/phpenv.git
$ phpenv/bin/phpenv-install.sh

以下を
~/.bash_profileに追記


export PATH="$HOME/.phpenv/bin:$PATH"
eval "$(phpenv init -)"


~/.bash_profileを再読み込み


$ source ~/.bash_profile


## php5.6.0のインストール


実行するコマンドはこれだけなのですが、この後いろんなエラーが出てインストールに失敗します。エラーの内容ごとに解決方法を追記します。


$ phpenv install 5.6.0


### bisonバージョン問題



configure:5267: WARNING: This bison version is not supported for regeneration of the Zend/PHP parsers (found: 2.3, min: 204, excluded: 3.0).

El Capitanにインストールされているbisonが
2.3と古いため起きる問題です。
2.7以上が必要な為下記コマンドを実行してインストールします。


$ brew tap homebrew/versions
$ brew install bison27
$ brew link bison27 --force

参考： 
[homebrew で標準のバージョンよりも古い bison をインストールする - Sarabande.jp](http://blog.sarabande.jp/post/53780323365)

またそのままだと2.3側を利用してconfigureしてしまうので下記ファイルの冒頭に 
YACC=/usr/local/opt/bison27/bin/bison を追記することで 
2.7 のbisonを利用するように指定します。


~/.phpenv/plugins/php-build/share/php-build/default_configure_options

参考：
[phpenvでphp5.5.7とかビルドするとbisonとかでおこなので、brewでどうにかするつもりが結局bison27のFormulaかく羽目になったやつ - uzullaがブログ](http://uzulla.hateblo.jp/entry/2013/12/29/165752)


### OpenSSL問題



configure: error: Cannot find OpenSSL's &lt;evp.h&gt;

素のEl Capitanではopensslがインストールされていないため発生します。必要パッケージをインストールします。


$ brew install openssl
$ brew install libxml2
$ brew link openssl --force
$ brew link libxml2 —-force

が下記のようにlinkに失敗します。


$ brew link openssl --force
Warning: Refusing to link: openssl
Linking keg-only openssl means you may end up linking against the insecure,
deprecated system OpenSSL while using the headers from Homebrew's openssl.
Instead, pass the full include/library paths to your compiler e.g.:
-I/usr/local/opt/openssl/include -L/usr/local/opt/openssl/lib

コンパイル時にパスを渡せばビルドすれば解決します。


$ PHP_BUILD_CONFIGURE_OPTS="--with-openssl=$(brew --prefix openssl) --with-libxml-dir=$(brew --prefix libxml2)" phpenv install 5.6.0


### パッケージ不足



configure: WARNING: You will need re2c 0.13.4 or later if you want to regenerate PHP parsers.
これは単純に 
re2c がないとの事なのでインストールすれば解決。


$ brew install re2c


# configure: error: jpeglib.h not found.
# configure: error: png.h not found.
# configure: error: mcrypt.h not found. Please reinstall libmcrypt.

こちらも同様に下記をインストールして解決。


$ brew install libjpeg
$ brew install limping
$ brew install libmcrypt

参考：
[Homebrewでphp-build及びphpenvインストール後に5.5系をインストールするための手順 - Qiita](http://qiita.com/tebonz/items/82ad7e57066650d43b28)

 

次のエラーは長いので
[gist](https://gist.github.com/hypermkt/e4a60fc93f733a48ee2157f6e23b2a73)で

参考：
[phpenv+php-build環境によるphpバージョン管理~Mac（Yosemite）編~ - Qiita](http://qiita.com/omega999/items/c5b1c177331f8d342efd)

エラーログを眺めるとmacro周りでwarningが発生している。ここからconfigureが正常に実行できないことが分かりautoconf, automakeが推測できるとのこと（友人談）。またEl Capitanには初期ではそれらが未インストール状態につき、ビルドに失敗する。なるほど・・・。


$ brew install autoconf
$ brew install automake

これでビルドが通ります。


## PHP 5.6.0への切り替え


下記でEl Capitanに標準でインストールされているPHP 5.5からPHP 5.6.0に切り替えができます。


$ phpenv versions
$ phpenv global 5.6.0
$ phpenv rehash

いや〜。なかなかしんどかった・・・。