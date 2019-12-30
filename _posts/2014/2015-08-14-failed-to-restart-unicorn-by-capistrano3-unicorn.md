---
layout: post
title: capistrano3-unicornでunicornを起動しようとしてハマった
post_id: 534
categories: 
- Capistrano
- Ruby
---

## 環境



*  capistrano (3.4.0)


*  capistrano3-unicorn (0.2.1)


## したかったこと



*  Capistranoでデプロイ時にunicornを再起動をする


## ハマったこと



*  bundle exec cap production deployしても
unicornがうんともすんとも言わない・・・。


*  上記の問題解決後、再度実行したら``pid=': Already running on PID:11705`エラーが発生して正常に再起動できない


### log/unicorn.stderr.log



#{deploy_to}/shared/bundle/ruby/2.2.0/gems/unicorn-4.9.0/lib/unicorn/http_server.rb:206:in `pid=': Already running on PID:11705 (or pid=/tmp/unicorn.pid is stale) (ArgumentError)
        from #{deploy_to}/shared/bundle/ruby/2.2.0/gems/unicorn-4.9.0/lib/unicorn/http_server.rb:135:in `start'
        from #{deploy_to}/shared/bundle/ruby/2.2.0/gems/unicorn-4.9.0/bin/unicorn:126:in `<top (required)>'
        from #{deploy_to}/shared/bundle/ruby/2.2.0/bin/unicorn:23:in `load'
        from #{deploy_to}/shared/bundle/ruby/2.2.0/bin/unicorn:23:in `<main>'


## 問題１：unicornがうんともすんとも言わない


capistranoのデプロイログを見ても、unicornが起動されたログが全くなく完全に無視されているような状態だった。一旦 
unicorn:startで直接起動しても、下記の通り何も起きなかった。


$ bundle exec cap production unicorn:start
DEBUG [96aa134d] Running /usr/bin/env [ -d /usr/local/rbenv/versions/2.2.0 ] on 
DEBUG [96aa134d] Command: [ -d /usr/local/rbenv/versions/2.2.0 ]
DEBUG [96aa134d] Finished in 2.405 seconds with exit status 0 (successful).

原因はrole設定の不一致っだった orz capistrano3-unicornの
[unicorn.rake](https://github.com/tablexi/capistrano3-unicorn/blob/master/lib/capistrano3/tasks/unicorn.rake#L5)によるとデフォルトで
appになっているが、自分が設定した
config/deploy/production.rbでは
app以外を設定しており、そこがずれていたからだった。


## 問題２：Already running on PID



capistrano3-unicornの
restartタスクではまず
startタスクを実行し、起動していなければ起動、起動済みなら何もしないという順序になっている。ここで起動をしようとするとunicornが既に起動済みだよ！と怒られてしまう・・・。

原因はまたしても設定の不一致だった orz 
[unicorn.rake](https://github.com/tablexi/capistrano3-unicorn/blob/a4adb59031b001a9cd8f9dce5f4beb3200c80f8d/lib/capistrano3/tasks/unicorn.rake#L3)の
unicorn_pidに応じてpidファイルが作られるのだが、


*  config/deploy/production.rb


*  set :unicorn_pid, "#{shared_path}/tmp/pids/unicorn.pid"


*  config/unicorn/production.rb


*  /tmp/unicorn.pid

と全く別の場所を参照していたのだ・・・。こちらもsharedに合わせることで解決。

地味な問題で数時間もハマってしまったが解決できてよかった。これでデプロイするたびにunicornが再起動してくれるので完璧だ。