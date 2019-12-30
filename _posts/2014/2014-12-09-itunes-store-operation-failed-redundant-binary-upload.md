---
layout: post
title: XcodeでValidate時に「iTunes Store operation failed redundant binary upload」言われた場合の対策
post_id: 476
categories: 
- iOS
- Xcode
---

[![09ed37cc-7d65-11e4-8fb4-35a1b1447c1a](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/12/09ed37cc-7d65-11e4-8fb4-35a1b1447c1a.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/12/09ed37cc-7d65-11e4-8fb4-35a1b1447c1a.png)

よく見ると詳細なエラーが表示されていた

Redundant Binary Upload.There are already exists a binary upload build with '1' for versi
切れてしまっているがこれを訳すと

バイナリーの重複アップロードです。既にVersion '1'としてビルドされたバイナリーが存在しています。

実はアプリ申請の過程で既に一回バイナリーは送信済みで、時間があったので追加で修正を加えた物を再送信しようとしたのであった。Xcode側にもVersionが存在し、そこが1.0のままだからエラーとなったようだ。


[![bd673c3a420b08828f8bdb1f266b878e](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/12/bd673c3a420b08828f8bdb1f266b878e.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/12/bd673c3a420b08828f8bdb1f266b878e.png)

なのでそこを1.0.1にバージョンアップすることでValidateが通り。送信出来た。