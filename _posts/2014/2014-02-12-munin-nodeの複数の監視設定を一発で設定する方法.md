---
layout: post
title: munin-nodeの複数の監視設定を一発で設定する方法
post_id: 221
categories: 
- munin
---

muninの監視項目を追加したい際に今まで下記のようにシンボリックリンクを手動で貼ってました。# ln -s /usr/share/munin/plugins/mysql_bytes /etc/munin/plugins/mysql_bytes
# ln -s /usr/share/munin/plugins/mysql_isam_space_ /etc/munin/plugins/mysql_isam_space_
# ln -s /usr/share/munin/plugins/mysql_queries /etc/munin/plugins/mysql_queries
# ln -s /usr/share/munin/plugins/mysql_slowqueries /etc/munin/plugins/mysql_slowqueries
# ln -s /usr/share/munin/plugins/mysql_threads /etc/munin/plugins/mysql_threads

手段としては間違っていないのですが、一個一個コマンドを実行して行くのはまー面倒い。会社の人にそんな事をつぶやいたら、munin-nodeに標準で梱包されているmunin-node-configureを使えば簡単かつ一発で設定出来ますよ！と教えてもらいました。

まずは下記のようにsuggestオプションを利用する事で、現在利用中と設定可能な物を一覧で表示できます。Usedが現在設定中。Suggestionsがまだ未設定だけど設定可能というもの。


# munin-node-configure -suggest
Plugin                     | Used | Suggestions                            
------                     | ---- | -----------                            
acpi                       | no   | no [cannot read []                     
amavis                     | no   | no                                     
apache_accesses            | no   | yes                                    
apache_processes           | no   | yes                                    
apache_volume              | no   | yes                                    
...ずらずら続く

設定可能な物を確認した後にshellオプションを利用すると設定用のシンボリックリンクを文字列で出力してくれます。これを一個一個実行すれば問題ないのですが、面倒いので一発で設定しちゃいます。


# munin-node-configure -shell
ln -s '/usr/share/munin/plugins/apache_accesses' '/etc/munin/plugins/apache_accesses'
ln -s '/usr/share/munin/plugins/apache_processes' '/etc/munin/plugins/apache_processes'
ln -s '/usr/share/munin/plugins/apache_volume' '/etc/munin/plugins/apache_volume'

下記のようにパイプでシェルに渡してしまえば、上記のシンボリックリンクコマンドが全て実行され一発で監視設定を追加できます。


# munin-node-configure -shell | sh


[@glidenote](http://blog.glidenote.com/)先生、ありがとうございます！