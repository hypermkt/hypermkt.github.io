---
layout: post
title: IPAの「安全なウェブサイトの作り方」改訂第7版を読んだ
post_id: 1184
categories: 
- セキュリティ
---

## 結論



*  Webアプリケーションエンジニアは、IPA 情報処理推進機構が公開している
[「安全なウェブサイトの作り方」改訂第7版](https://www.ipa.go.jp/files/000017316.pdf)を一度は読むことをオススメする

    
*  本書を読むことでIPAに届け出が多く、影響度が大きい脆弱性の概要・対策方法を一通り学ぶことができる


## はじめに


最近Webセキュリティについて改めて学び直す必要があると感じることが多くなってきた。そこでIPA 情報処理推進機構が公開している
[「安全なウェブサイトの作り方」改訂第7版](https://www.ipa.go.jp/files/000017316.pdf)を読んだ。


## 安全なウェブサイトの作り方とは


IPAのホームページ上にこのように説明がある。


>「安全なウェブサイトの作り方」は、IPAが届出 を受けた脆弱性関連情報を基に、届出件数の多かった脆弱性や攻撃による影響度が大きい脆弱性を取り上げ、ウェブサイト開発者や運営者が適切なセキュリティを考慮したウェブサイトを作成するための資料です。


資料はPDFとして無料で公開されており、誰でも閲覧することができる。また「別冊：安全なSQLの呼び出し方」の資料も同様に無料で公開されている。これは、SQLインジェクションのための資料ではあるが、SQLインジェクションの被害件数の多さとその影響度合いが大きいことから、別資料として詳細に解説されているものと思われる。


## 安全なウェブサイトの作り方を読んで


本書は、IPAに届け出があった脆弱性から件数が多くかつ危険度が高いものが選定されている。そのため、これらを一通り学ぶことで、必要最低限の知識を習得できる。

本書は各脆弱性について、概要、脅威、届出状況とまず最初に脆弱性に関する脅威について説明がある。これにより脆弱性の概要と実態を知ることができる。


### 根本的解決と保守的対策の選択


次に解決策として根本的解決、保守的対策の２つの視点で解決方法が提案される。本書においてここが一番重要な点である。

根本的解決、保守的対策は以下のように説明がされている。


*  根本的解決

*  「脆弱性を作り込まない実装」を実現する手法

    
*  脆弱性を狙った攻撃が無効化されることを期待

    
*  保守的対策

*  攻撃による影響を軽減する対策

    
*  攻撃される可能性を低減する

    
*  攻撃された場合に、脆弱性を突かれる可能性を低減する

    
*  脆弱性を突かれた場合に、被害範囲を最小化する

    
*  被害が生じた場合に、早期に知る

当たり前ではあるが、根本的対策を実施して、脆弱性を無効化するのが一番良い。しかし、運用中のシステムにおいて、現場のリソース、稼働状況、時間などの制約から根本的解決が実施できない場合がある。そこで、保守的対策として攻撃を軽減する対策が提案されている。これも一つの選択肢であるので、知っておくべきと感じた。


### 脆弱性を作り込まないためには正しい知識と実装が必要


理想的には脆弱性を最初から作り込まないのが良い。そのためには脆弱性に対する正しい知識と理解を持って、実装する必要があることがわかった。ただ開発において考えることはセキュリティだけではない。設計、実行速度など様々なことを考えながら開発をすすめるので、絶対に脆弱性を埋め込まないというのは難しい。

脆弱性を埋め込まない開発方法の一つとして、チーム全体でWebセキュリティ力をあげて、コードレビューなどで脆弱性を作らない体制も一つの方法と感じた。


### 脆弱性対策には正しい知識と判断が必要


世の中に存在する脆弱性を全て理解して一切埋め込まないようにするのはさすがに難しい。ただ可能な限り減らすことは前項で意見を述べた。大切なのは脆弱性を見つけたときの行動である。脆弱性対策には


*  根本的解決

    
*  保守的対策

の２つの選択肢がある。加えてそれにサービス仕様も必要である。脆弱性対策の方法とサービス仕様、そしてその他状況（リソース、被害状況有り無し、影響範囲）などを考慮して総合的に判断が必要である。色んな変数が絡み合うことで迷いが生じることもあるが、正しい知識を持つことで、その変数はある程度固定化されることにより、判断がしやすくなると個人的には思う。なので、引き続きWebセキュリティに関する知識は学んでいかなくてはいけないと感じた。


## 終わりに


「安全なウェブサイトの作り方」を通じて影響度・発生頻度の高い脆弱性の概要・対策を一通り学ぶことができた。特に根本的解決、保守的対策の２つの観点を得られたのが大きい。引き続きWebセキュリティについて学んでいきたい。


## 参考



*  [安全なウェブサイトの作り方：IPA 独立行政法人 情報処理推進機構](https://www.ipa.go.jp/security/vuln/websecurity.html)

    
*  [「安全なウェブサイトの作り方」改訂第7版](https://www.ipa.go.jp/files/000017316.pdf)