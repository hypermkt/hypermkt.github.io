---
layout: post
title: Sequel Proで選択した行をMarkdown出力するバンドルを作りました
post_id: 673
categories: 
- SequelPro
---

元々実装自体は1年前に書いてはいたんですが、ずっとgistに放置したままになったので簡単にインストールしやすいようにしました。##概要



*  Sequel ProはMac用MySQLクライアントで、接続設定をすると簡単に検索やSQLを実行できるとても便利なソフトウェアです。


*  GitHub issueに貼りやすくするためSequel Proの検索結果をMarkdown形式でコピーできるバンドル（プラグインのようなもの）を実装しました


## インストール方法



[hypermkt/CopyAsMarkdownForSequelProBundle](https://github.com/hypermkt/CopyAsMarkdownForSequelProBundle)をgit cloneして 
copyAsMarkdown.spBundle をダブルクリックするだけです。


## 使い方



*  検索結果行を選択


*  メニューから「バンドル > copyAs > copyAsMarkdown」を選択するとコピーされます


*  ![copy_as_markdown](https://hypermkt-blog.lolipop.io/wp-content/uploads/2016/04/copy_as_markdown.png)


*  後は貼り付けるだけです！


id|method|accessed_at
---|---|---
15207807|GET|2016-01-25 00:00:00
15207808|GET|2016-01-25 00:00:00
15207811|GET|2016-01-25 00:00:01

検索結果をMarkdownの表として整形して残したい時に便利です！