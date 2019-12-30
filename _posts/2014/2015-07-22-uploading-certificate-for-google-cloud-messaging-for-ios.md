---
layout: post
title: Google Cloud Messaging for iOSの設定画面でAPNS証明書のアップロードにハマった
post_id: 527
categories: 
- Google Cloud Messaging
- iOS
---

ここ数日Google Cloud Messaging for iOSの設定画面からApple Developer Centerから発行した開発・本番用のAPNs証明書をアップロードした際に下記エラーがずっと表示されて、本番側の証明書のアップロードに失敗していた。


[![error-message-on-gcm](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/error-message-on-gcm.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/error-message-on-gcm.png)The certificate environment did not match.
Ensure that you got the right development or production APNS certificate.

エラー文言を見る限りでは証明書の環境がマッチしていないとのことで、何度も見なおしたが間違いなく本番の証明書であり、何度やり直してもずっと失敗していた・・・

が、やっと原因が分かった。

Apple Developer CenterのAPNS証明書を作成する画面をよく見直すと


>A separate certificate is required for each app you distribute.


と書いてあり、別々の証明書（ここではCSR）が必要ですとのこと。そういえば、開発・本番のAPNS証明書を発行する際にどちらも同じCSRを利用していたが、どうやらこれが原因だということがわかった。MacのキーチェーンアクセスからCSRを発行しなおして、それを利用して本番側のAPNS証明書を発行しなおしたら


[![success-on-gcm](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/success-on-gcm.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/success-on-gcm.png)

正常にアップロードすることができた。無事に解決。