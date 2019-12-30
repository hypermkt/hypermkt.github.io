---
layout: post
title: Apache httpd.confのBufferLogsでハマった話
post_id: 68
categories: 
- Apache
- Centos
---

[![Apache-Logo](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/Apache-Logo-300x206.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/01/Apache-Logo.png)

この設定のおかげで三時間近く悩みました。access_logの書き込みの挙動がやけに不規則で再起動をすると一気に書き込まれたりとおかしな〜とず〜と悩んでました。

友達に聞いた結果このBufferLogsが原因だと分かりました。


## [BufferLogs](http://www.apache.jp/manual/ja/mod/mod_log_config.html#bufferedlogs)



>BufferedLogs ディレクティブを使うと mod_log_config の挙動が変化して、 複数のログを書き出す際に、それぞれのリクエスト処理後毎に 書き出すのではなく、いったんメモリに蓄えてから、 まとめてディスクに書き出すようになります。 この結果ディスクアクセスがより効率的になり、 高いパフォーマンスの得られるシステムもあるでしょう。 このディレクティブはサーバ全体で一度だけ設定できます; バーチャルホストごとに設定することはできません。


上記の通りBufferLogs Onだとログはリアルタイムで書き込まれず、一定量溜まるまで書き込まれない挙動に変わるとの事。

なので、自分はアクセスログの書き込みが遅延してると勘違いしてましたが、遅延というのは間違いでした。

説明の通りDisk/IOが減るのでサーバー負荷も下がるので設定はOnでも問題ないですね。

いや〜、解決してよかった！