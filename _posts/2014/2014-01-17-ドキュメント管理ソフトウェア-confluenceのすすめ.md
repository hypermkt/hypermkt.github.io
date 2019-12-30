---
layout: post
title: ドキュメント管理ソフトウェア Confluenceのすすめ
post_id: 96
categories: 
- Confluence
---

[![logoConfluencePNG](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/logoConfluencePNG-300x78.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/logoConfluencePNG.png)


## 概要


アプリケーションやウェブサイトの開発の際に仕様、キャンペーンの企画などをGitHub/TracのissueやWikiなどで管理するケースが多いと思います。ですが、GitHub/TracのWikiはどちらかと技術者向けのソフトウェアであり一般の人が使うには少しハードルが高いと感じています。またissueやticketは数が増えると埋もれてしまい探すのが手間です。

そこで誰もが簡単に情報共有ができるドキュメント管理ソフトウェアのConfluence（コンフルエンス）の紹介をしたいと思います。Confluenceはオーストラリアのatlassian社が開発しているソフトウェアでその他にJIRA/Bitbucketなども開発している有名なソフトウェア会社です。とあるレビューサイトの仕様や施策の企画・結果等は全てConfluenceで管理しています。


## Confluenceとは？


この動画をご覧ください。





## Confluenceを使うメリット



*  Markdown/Wiki記法など全く知らなくてもホームページ作成ソフトウェアのように操作が出来る。つまり一般的なITリテラシーがあれば誰でも使える。


*  コメント機能、メンション機能あり。


*  各ドキュメントを階層階層構造で管理できる

これだけでも十分なメリットだと思います。


## 契約


契約形態にオンデマンド型（ASP)とダウンロード型の２種類があります。


[![conflucence_ondemand](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/conflucence_ondemand.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/conflucence_ondemand.png)


[![conflucence_download](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/conflucence_download.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/conflucence_download.png)

オンデマンド型は所謂ASPです。サーバー代は自腹となりますが、長期的に考えるとダウンロード版がお得でオススメです。特に小規模なチームなら１０ユーザー１０ドルなので、一回１０ドル払えばずっとConfluenceが使えます。


## インストール方法


実際にインストールした時に履歴を記録していなかったので詳細は
[こちら](https://confluence.atlassian.com/display/DOC/Confluence+Setup+Guide)をご確認ください。英語のマニュアルですがこの通りに従えばインストールできます。


## インストール先サーバーの要件



*  [Server Hardware Requirements Guide](https://confluence.atlassian.com/display/DOC/Server+Hardware+Requirements+Guide)によると


### 5 Concurrent Users



*  2GHz+ CPU


*  512MB RAM


*  5GB database space

とのことです。また
[システム要件](https://confluence.atlassian.com/display/DOC/System+Requirements#SystemRequirements-ConfluenceHardwareRequirements)によるとJava/PostgreSQLが必要との事なので共用レンタルサーバーでのインストールは難しいので、メモリ1G CPU Xeon 2.4Ghz*2 月額1000円のVPSにインストールしています。


## 最後に


アプリケーションやウェブサイトを開発する際は企画、開発、運用、改善のフローが周期があって成長して行きます。誰もが簡単にドキュメントが作成出来るConflunceを使えば、ディレクターからの施策の提案、開発者の仕様のまとめ、広告の成果などが全て一つにまとめられます。チーム内のスキルを高め、各種便利なツールを使えばWikiなどでも問題ないですが、もう一つの選択肢としてConflunceはオススメです！（回し者っぽいですね