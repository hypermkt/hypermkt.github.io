---
layout: post
title: DeployGateにテスターを追加する方法
post_id: 434
categories: 
- DeployGate
- iOS
---

## 前提



*  Apple Developer Programに契約済。但しテスターの人は不要。


*  対象アプリ用のDeployGateアカウントに登録済み。テスターアカウントは別途登録が必要。


## 流れ



### Apple Developer Program



Devicesに対象iPhoneを登録

XCode > Window > Devicesから自分のiPhone端末を選択し、UUIDを取得します。

[![1](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/1.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/1.png)

Apple Developer Program > Member Center > Certificates, Identifiers & Profiles > Certificates > Devices all > +（プラスマーク）を選択し、先ほど取得したUUIDを入力


[![2](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/2.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/2.png)

登録成功すると一覧画面に表示されます。


[![3](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/3.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/3.png)


*  プロビジョニングプロファイルに対象デバイスを入れてプロビジョニングプロファイルをダウンロードし直す


### DeployGate


アプリ用DeployGateにログイン後、テスター用DeployGateのアカウントのメールアドレスを入力して招待します。


[![deploygate](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/deploygate.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/deploygate.png)


### iPhone


招待メールが届いたら、メール本文内のURLをクリックします。URLを開くと下記ページが表示されるので「DeployGateをインストール」をクリックします。


[![01_install_deploy_gate](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/01_install_deploy_gate.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/01_install_deploy_gate.png)

プロファイルのインストールが聞かれるのでそのままインストールします。


[![02_install_deploy_gate_profile](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/02_install_deploy_gate_profile.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/02_install_deploy_gate_profile.png)

インストールするとiPhone上にDeployGateアイコンが表示されます。


[![03_deploy_gate_icon](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/03_deploy_gate_icon-e1413727212939.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/03_deploy_gate_icon-e1413727212939.png)

DeployGateアイコンをタップするとアプリ一覧が表示され、これでアプリのダウンロードが出来るようになり準備完了です！


[![04_ready_to_download](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/04_ready_to_download.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/10/04_ready_to_download.png)