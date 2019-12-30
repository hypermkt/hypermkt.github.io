---
layout: post
title: 全ページをSSL化する方法
post_id: 665
categories: 
- SEO
---

## 概要


先日
[あにこれ](https://www.anikore.jp)の全ページSSL化の導入が完了しました。本件ではその経験を元に全ページをSSL化する際の確認事項・手順・注意点等をまとめます。


## なぜ全ページSSL化するのか


SEOとセキュリティ対策が目的です。「
[Google ウェブマスター向け公式ブログ: HTTPS をランキング シグナルに使用します](http://googlewebmastercentral-ja.blogspot.jp/2014/08/https-as-ranking-signal.html)」にある通りGoogle公式ブログでランキングアルゴリズムに僅かながら寄与すると明言しています。暗中模索のSEO対策の中、比較的効果が期待できる方法につき、やらない理由は無いです。


## 全ページSSL化とは



*  httpsで表示するページ内の全ての既存サーバー内の静的ファイル、外部リソースファイルの読み込み、通信を
httpsに切り替えること


*  httpでアクセスしたら自動的に
httpsに301リダイレクトされること


## やらないこと



*  外部サイトへのAタグのURLはhttpのままでも問題ありません。


*  【個人的にオススメしない】HSTS


*  初回SSL導入時はオススメしません。これを初回に実施してしまうとトラブル発生時に
httpに戻すのが大変になります。


## 確認事項



### Webサーバー


まずはWebサーバー本体にSSL証明書の導入が必要です。利用しているウェブサーバーのミドルウェアに応じてインストールしましょう。


### 静的ファイルへのパス


画像、JavaScript, CSSファイルなどの静的ファイルのパスをhttpsで参照できるように変更する必要があります。方法は２つあります。


*  ファイルへのパスをhttpsから記述する


*  例) 
<img src="https://www.anikore.jp/logo.png" />


*  URIスキームを削除してhttp/https両方に対応させる


*  例) 
<img src="//www.anikore.jp/logo.png" />


### 画像サーバー



img.hogehoge.jp等の独自の画像サーバーを構築して利用している場合は、対象のサブドメインにもSSL証明書の導入が必要です。Webサーバー同様に利用しているミドルウェアに応じてインストールしましょう。


### CDN


Akamai/CloudFrontを利用している場合はhttp非対応のプランもあるので、各サービスのマニュアルを確認しましょう。


### 広告/アフィリエイト


広告やアフィリエイトシステムでは各社が計測や広告出力用のJavaScript, 画像を配信しており、https化は各社のサーバーに依存します。


https対応可能

下記はhttpsに対応しているので、URLを変えることで対応できます。


*  Google AdSense


*  楽天アフィリエイト


*  Nend


https未対応


*  A8


*  A8に問い合わせた所、画像は自分のサーバーから配信して問題ないとのことです。


### fluentd


Apache/Nginx共に初期設定ではアクセスログの出力先が分かれているので、ログを転送している場合は合わせてfluentdの設定を調整してください。


## 手順


各環境・状態に応じて順序は変わりますが、ざっくりと手順とまとめます。


### 事前リリース可能



*  Web/画像サーバーにSSL証明書をインストール


*  静的ファイル・広告類をhttpsに切り替える


*  fluentdの設定変更


### 全ページSSL化



*  全ページで301リダイレクト


### 切替後に要対応



*  Google Analyticsのプロパティ名を
httpsに変更する


*  Google Analyticsのプロパティ設定のデフォルトURLを
httpsに変更する


*  SearchConsoleに
httpsの新URLのプロパティを新規追加する


*  SearchConsoleで
httpsの新URLが列挙された新サイトマップXMLを送信する


## まとめ


当初は簡単かつすぐに出来ると考えてましたが、作業を始めて見ると既存のコード内に
httpの直書き(反省)があったり、他サーバーへの影響(見積もりが甘い)もあり、アプリケーション・インフラを横断して対応が必要で地味に大変でした。

ただ念願の全ページSSL化が対応でき、今後オーガニック検索の流入がどう変化するのか楽しみです。


## 参考にしたサイト



*  [HSTS (HTTP Strict Transport Security) の導入 - Qiita](http://qiita.com/takoratta/items/fb6b3486257eb7b9f12e)


*  [mod_rewriteでHTTP / HTTPSの切り替え - Qiita](http://qiita.com/gotohiro55/items/7daa988db23a5a8355c1)


*  [.htaccessでHTTPアクセスをSSLでリダイレクト(逆もアリ) - hogehoge foobar Blog Style5](http://d.hatena.ne.jp/mrgoofy33/20100914/1284414817)