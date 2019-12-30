---
layout: post
title: vagrant pluginをインストールしようとするとnokogiriエラーがでてインストールできない時の対策
post_id: 489
categories: 
- Vagrant
---

下記エラーが表示された場合は$ vagrant plugin install vagrant-librarian-puppet
Installing the 'vagrant-librarian-puppet' plugin. This can take a few minutes...
Bundler, the underlying system Vagrant uses to install plugins,
reported an error. The error is shown below. These errors are usually
caused by misconfigured plugin installations or transient network
issues. The error from Bundler is:

An error occurred while installing nokogiri (1.6.5), and Bundler cannot continue.
Make sure that `gem install nokogiri -v '1.6.5'` succeeds before bundling.

普通にnokogiriをインストールしても対応できないので


$ gem install --install-dir ~/.vagrant.d/gems nokogiri -v '1.6.5'

でインストールしてあげればエラーが解消できます。