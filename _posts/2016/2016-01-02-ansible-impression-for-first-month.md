---
layout: post
title: Ansibleを一ヶ月使ってみての感想
post_id: 630
categories: 
- Ansible
---

## 結論



*  最初から
[入門Ansible [Kindle版]](http://www.amazon.co.jp/%E5%85%A5%E9%96%80Ansible-%E8%8B%A5%E5%B1%B1%E5%8F%B2%E9%83%8E-ebook/dp/B00MALTGDY)を買って読んでおけば良かった！


*  ちまたで言われているほどシンプルでも簡単でも無かった。


*  それでもやっぱりPuppet/Chefより学習コストは低いので今後も使いたい。

以上！


## 最初から入門Ansible [Kindle版]を買って読んでおけば良かった


3週目位に存在を知ったのですが、最初から読んでおけば良かったです。この本を読めば基本的なことは全て学べます。


### ベスト・プラクティスには黙って従おう


さくらのナレッジブログでWordPressを動かす環境構築を例に分かりやすく解説してくれます。本来は
[本家のBest Practices](http://docs.ansible.com/ansible/playbooks_best_practices.html)を読むべきですが、やっぱり日本語の方が読みやすいですね（すません）。日本語で感触が掴んでから英語版を読むと良いです。


*  [実践！Ansibleベストプラクティス（前編） - さくらのナレッジ](http://knowledge.sakura.ad.jp/tech/3084/)


*  [実践！Ansibleベストプラクティス（後編） - さくらのナレッジ](http://knowledge.sakura.ad.jp/tech/3086/)


## ちまたで言われているほどシンプルでも簡単でも無かった。


よくAnsibleはシンプルで簡単に使えるとどのサイトにも書かれていますが、一ヶ月経っての正直な感想は全くそうではないです。


### changed_when, failed_whenの意味が分からなかった


shellやcommandモジュールなど一部のモジュールでは単なる状態確認でも常にchanged扱いになってしまいます。単にshellを実行しているだけなので、OKと表示してほしいことがあります。その際にchanged_whenを使うのですが、この辺の判別を記述する方法が独特かつマニュアルが少ないので最初混乱しました。


### モジュール数が463個と多い！


モジュール数が思っていた以上に多かったことです。 
[こちら](http://docs.ansible.com/ansible/list_of_all_modules.html))にAnsibleのtaskで使える全モジュール一覧があるんですが、463個あります・・・。プログラミング言語の関数に比べれば大したことはないですが、「シンプル」「簡単」という謳い文句を勝手に信じていたので、結構多かったんだねという印象でした。ただ実際この一ヶ月で使ったのは下記の10個だけでした。ただこの10個があればだいたい用は足ります。


*  service


*  yum


*  gem


*  copy


*  template


*  user


*  file


*  replace


*  shell


*  command


## それでもやっぱりPuppet/Chefより学習コストは低いので今後も使いたい


（言うほどPuppet/Chefを使ったことがあるのか！と怒られそうですが）
今後も使いたいです。総じてAnsibleの良いと感じたところは


*  最低10個のモジュールの使い方を覚えればどうにかなる


*  Best Practicesに従っておけば煩雑になりにくい


*  インストールが簡単なのは最高。ssh接続ができれば対象サーバーに対してすぐに実行できる


*  個人的にはこれが一番の理由でもあります。今までChefのインストールに相当苦しめられたので・・・。

ということで年末年始休暇はAnsible一色で終わってしまった。