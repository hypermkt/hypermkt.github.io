---
layout: post
title: Vue.js Tokyo v-meetup #4でLTしてきました
post_id: 1013
categories: 
- Vue.js
---

## はじめに


2017年7月7日(金) LINE株式会社オフィスで開催された「
[Vue.js Tokyo v-meetup #4](https://vuejs-meetup.connpass.com/event/58071/)」でLTしてきました。 当初は、「Vue.jsで作る音声レコーダー」というユニットテストとは全く関係ない事を話す予定でした。


## なぜユニットテストの話にしたのか


とある日同僚の 
[@Joe_noh](https://twitter.com/Joe_noh) が「Vueコンポーネントのテストをきっちりやっているの、世界で３人くらいしかいないのでは」と言っていました。話を聞くと、業務でVue.jsを利用し始めたが、コンポーネントのテストが辛い、事例やサンプルの情報が少ないとのことでした。確かに自分も以前から同じことを感じていました。

その状況の原因としては、「The Progressive JavaScript Framework」所以なのではと考えていました。Progressive とは、「アプリケーションの段階的な要求変化に応じて問題解決できる方法を提供できる。必要になった時に問題解決するライブラリを適宜導入して問題を解決する」という意味です。Vue.js本体はフルスタックなフレームワークではなく、ビュー層に焦点が当てられてたフレームワークです。それゆえ、
[ドキュメントのテストの項目](https://jp.vuejs.org/v2/guide/unit-testing.html#%E3%83%86%E3%82%B9%E3%83%88%E3%83%84%E3%83%BC%E3%83%AB%E3%81%A8%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97)にも、単純な例やDOM更新の例しかありません。それ自体は、ライブラリの方針なので問題ではありません。

しかし現場は違います。RouterもHttpもStoreも利用します。それらが連携して一つのアプリケーションとなります。そういった状況で簡単にテストが書けるライブラリが必要と感じていました。
[@Joe_noh](https://twitter.com/Joe_noh) から
[avoriaz](https://github.com/eddyerburgh/avoriaz)、
[vue-test-utils](https://github.com/vuejs/vue-test-utils)の存在を教えてもらい、他の人も同じことを考えていることが分かりました。

であれば、今回のMeetupで、テストを書くのが辛い状況を共有し、テストに関することをもっとアウトプットしてもらおう！と考え、ユニットテストの話をすることにしました。


## 発表資料





現時点(7/9 17:00)で
[133ブクマ](http://b.hatena.ne.jp/entry/s/speakerdeck.com/hypermkt/vuekonponentofalseyunitutotesuto)頂き大変嬉しいです。それだけテストに関する需要や共感を頂けたものと感じています。


## LINE株式会社 新宿オフィス


2017年4月にヒカリエからミライナータワーに引っ越した新オフィスです。今回写真撮影NGとのことでしたので、有名な
[941さんのブログ](http://blog.kushii.net/archives/2048602.html)をご覧ください。こちらのオーディトリアムと呼ばれるセミナールームでした。スーパーオシャレでした。


## 懇談会



*  Vue.jsを始めたばっかり


*  テストって必要なんですか？


*  コンポーネントのロジックはテストしていたが、ビューはしていなかった。ビュー層のテストが出来るようになって良い。


*  vue-routerを使っているが、モックを作るのがめんどい


*  今日紹介されたライブラリは初めて聞いた


*  vue-test-utilsに期待

などいろんなコメントをもらいました。


## おわりに


情報提供、発表のきっかけを作ってくれた 
[@Joe_noh](https://twitter.com/Joe_noh), 資料のレビューをしてくれた 
[@inouetakuya](https://twitter.com/inouetakuya), 
[@tsuchikazu](https://twitter.com/tsuchikazu)の三人に感謝です。

また登壇時に大切なことを言い忘れていましたが、
何でも良いのでテストの実装例をアウトプットしてもらえると嬉しいです！