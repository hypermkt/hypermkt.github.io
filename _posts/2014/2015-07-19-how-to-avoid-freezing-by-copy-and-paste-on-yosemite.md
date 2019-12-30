---
layout: post
title: Mac Yosemite上のChromeでショートカットのコピー＆ペーストをすると固まる問題の一時的回避法
post_id: 502
categories: 
- Mac
---

[caption width="500" align="alignnone"]
![](http://blog.naosan.jp/wp/wp-content/imports/photos/uncategorized/2013/03/30/20100827005924.jpg) レインボーカーソル[/caption]

YosemiteにアップグレードしてからChrome上でコピー（⌘+C）、ペースト（⌘+V）をするとレインボーマークが表示されて数秒待たさせる現象がかなりの頻度で発生するようにあった。

調べるとChronium側にもissueが立っていて問題が報告されているが、どうやら日本語のIMEが関係しており、日本人のみで問題が発生している様子。*[Mac上のChromeでショートカットキーでコピー/ペーストをすると数秒待たされる問題 （未解決） - Qiita](http://qiita.com/shrkw/items/9388301ad43f1cfe3f05)


*  [Issue 473850 - chromium - Chrome hangs on copying/pasting text in a input field on Mac - An open-source project to help move the web forward. - Google Project Hosting](https://code.google.com/p/chromium/issues/detail?id=473850)

そこでYosemiteに標準でインストールされている日本語入力とChromeとの相性に問題があることがわかったので、
[Google日本語入力](https://www.google.co.jp/ime/)をインストールして試したところ、今のところ固まる問題は起きなくなった。あくまで一つの回避方法であり根本的な問題解決にはなってないが、１日に何十回と待たされるよりかはだいぶマシである。

また懸念点としてGoogleにパスワードなどの入力内容が送信されているのでは？という心配するが、下記の通り公式で
[送信することはない](https://support.google.com/ime/japanese/answer/166771?hl=ja)と明言されているのでおそらく大丈夫。


>入力した文字や文章がGoogle に送信されることはありません。
  インストール時、またはプロパティ画面の[その他]タブにある[使用統計情報と障害レポート]のチェックボックスをオンにした場合には、お客さまがご利用のOS情報、カスタマイズ情報、打鍵数などの統計情報、クラッシュレポートがGoogleに送信されますが、ここでも、Google日本語入力を通じて入力された単語や文章が送信されることはありませんのでご安心ください。


ということで、もし同様な問題に悩んでいる人がいたら、Google日本語入力を試してみてください。