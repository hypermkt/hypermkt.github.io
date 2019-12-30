---
layout: post
title: ntpd monlist脆弱性の対策法
post_id: 225
categories: 
- Centos
- ntp
---

[JPCERTよりntpdの脆弱性](http://www.jpcert.or.jp/at/2014/at140001.html)の報告があった


>NTP Project が提供する ntpd の一部のバージョンには、NTP サーバの状態を確認する機能 (monlist) が実装されており、同機能は遠隔からサービス運用妨害 (DDoS) 攻撃に使用される可能性があります。
  
  NTP は、通常 UDP を使用して通信するため、容易に送信元 IP アドレスを詐称することができます。また、monlist 機能は、サーバへのリクエストに対して大きなサイズのデータを送信元 IP アドレスへ返送するため、攻撃者は攻撃対象の IP アドレスを送信元 IP アドレスに偽装した問い合わせパケットをNTP サーバに送信することで、大きなサイズのデータを攻撃対象 (Web サイトなど) に送りつけることができます。
  
  ntpd 4.2.7p26 より前のバージョン
   *) 安定版の 4.2.6.x は全て影響を受けます。


全部ってことですね・・・

バージョンを調べた所見事にビンゴ。

対処方法としては
1. ntpdを停止する
1. ntpdを起動させる場合は/etc/ntp.confにdisable monitorを記述してmonlist機能をOFFにする
とのことでした。

考えた結果、そもそもntpdを常駐しておくほどの事もなく1日1回時間補正をしてくれればいいので、ntpdは停止し/etc/cron.daily/ntp.cronを追加し、毎朝時間補正をするようにしました。

chefに下記を追加


template "ntp.cron" do
  path "/etc/cron.daily/ntp.cron"
  owner "root"
  group "root"
  mode "755"
  source "/etc/cron.daily/ntp.cron.erb"
end


#!/bin/sh
/usr/sbin/ntpdate ntp.nict.jp

runlistに追加して適用！終わり！