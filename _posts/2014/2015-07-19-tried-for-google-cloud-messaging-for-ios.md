---
layout: post
title: Google Cloud Messaging for iOSのサンプルを試してみた
post_id: 507
categories: 
- Google Cloud Messaging
- iOS
- Xcode
---

## 環境



*  MacOS X Yosemite 10.10.4


*  Xcode 6.4


*  iPhone5(実機。シュミレーターでは出来ません。)


## Google Cloud Messaging for iOSとは？



*  **無料**
で利用できるプッシュ通知サービスです。


*  元々はAndroidのみの対応でしたが、2015年5月に行われたGoogle I/O 2015にてiOSの対応も発表されました。


## サンプルを試してみた



[公式サイト](https://developers.google.com/cloud-messaging/ios/start?hl=ja&ver=swift)によると下記の手順となっています。


*  プロジェクトの用意


*  設定ファイルの用意


*  プロジェクトに設定ファイルを追加


*  サンプルを実行

では始めます。


### プロジェクトの用意


上記の通り、実行すると下記となり。Xcodeが自動的に起動します。


$ bundle exec pod try Google
Updating spec repositories

CocoaPods 0.38.0 is available.
To update use: `sudo gem install cocoapods`

For more information see http://blog.cocoapods.org
and the CHANGELOG for this version http://git.io/BaH8pQ.


Trying Google
1: Samples/admob/AdMobExample.xcodeproj
2: Samples/analytics/AnalyticsExample.xcodeproj
3: Samples/appinvites/AppInvitesExample.xcodeproj
4: Samples/gcm/GcmExample.xcodeproj
5: Samples/signin/SignInExample.xcodeproj
Which project would you like to open [1-5]?
4
Performing CocoaPods Installation

CocoaPods 0.38.0 is available.
To update use: `sudo gem install cocoapods`

For more information see http://blog.cocoapods.org
and the CHANGELOG for this version http://git.io/BaH8pQ.

Installing GGLInstanceID (1.0.0)
Installing Google (1.0.7)
Installing GoogleCloudMessaging (1.0.3)
Installing GoogleInterchangeUtilities (1.0.0)
Installing GoogleNetworkingUtilities (1.0.0)
Installing GoogleSymbolUtilities (1.0.0)
Installing GoogleUtilities (1.0.1)

[!] Please close any current Xcode sessions and use `GcmExample.xcworkspace` for this project from now on.
Opening '/private/var/folders/_p/wq04phmd1x7g4clvhlxx_8zw0000gn/T/CocoaPods/Try/Google/Samples/gcm/GcmExample.xcworkspace'


## 設定ファイルの用意



[Enable Google Services for your app](https://developers.google.com/mobile/add)ページから取得できます。


Pick a platformボタンを押します。


[![gcm-01](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-01.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-01.png)


Cloud Messagingを選択して、
Generate configuration filesボタンを押します。


[![gcm-02](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-02.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-02.png)

開発用、本番用APNs証明書をアップロードする箇所が２箇所ありますが、サンプルを試すだけなので開発側だけでアップロードします。


[![gcm-03](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-03.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-03.png)


plistファイルができたらダウンロードして下さい。


[![gcm-04](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-04.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm-04.png)


### プロジェクトに設定ファイルを追加


ダウンロードした GoogleService-Info.plistファイルをドラッグアンドドロップでプロジェクトのルート直下に置きます。


[![choose_options_for_adding_files](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/choose_options_for_adding_files.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/choose_options_for_adding_files.png)


[![google-service.plist](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/google-service.plist_.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/google-service.plist_.png)


### 実行前に調整


このまま実行すると下記エラーが発生してしまいます。


[![IMG_4261](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/IMG_4261.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/IMG_4261.png)

これを回避するためにBundle IdentiferをAPNs証明書を発行したBundle Identifierに修正し、またteamも正しいものに設定してください。


[![bundle_id](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/bundle_id.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/bundle_id.png)


### サンプルを実行


再度サンプルを実行し直すと下記のようにGCMへの登録成功のメッセージが表示されます。Xcodeのログにトークンが記載されているのでメモしてください。


[![IMG_4262](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/IMG_4262.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/IMG_4262.png)

では次にプッシュ通知を送信する準備をします。TargetよりGcmServerDemoを選択し実行してください。


[![gcm_server_demo](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm_server_demo.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/gcm_server_demo.png)

実行したらapiKeyと先ほどログから取得したRegistrationKeyを入力し、Send Notificationボタンを押してください。


[![demo](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/demo.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/demo.png)

しばらくすると実機に下記メッセージが送信されれば成功です。


[![IMG_4263](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/IMG_4263.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2015/07/IMG_4263.png)

自分用のまとめ記事につき、APNsの証明書の取得など省いてしまいわかりづらいところもあるかもしれませんが、Google Cloud Message for iOSを試される際はご参考にしてください。