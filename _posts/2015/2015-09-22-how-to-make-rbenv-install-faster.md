---
layout: post
title: rbenv installする際にrdoc/ri生成を省略化してビルドを速くする
post_id: 539
categories: 
- Ruby
---

CONFIGURE_OPTS="--disable-install-rdoc" rbenv install -v 2.2.3

と
CONFIGURE_OPTS="--disable-install-rdoc"を追記することでrdocとriを省略することが出来ます。


### CONFIGURE_OPTSとは？


rubyビルドのパラメーターです。

[sstephenson/ruby-build](https://github.com/sstephenson/ruby-build#special-environment-variables)


>CONFIGURE_OPTS lets you pass additional options to ./configure.


上記によると 
./configureにオプションを渡すことが出来ます。


### --disable-install-rdocとは？



[How to install ruby without documentations - Qiita](http://qiita.com/maangie/items/532809d78b4fade0d487)

configureオプションに
--disable-install-rdocを渡すことによりRDocの生成をやめるとのことです。

Rdocの生成で画面がほぼ止まってしまい本当にビルドしているのかどうかわからない状況に良くなっていたのですが、これのおかげでその心配もなくなり助かります。