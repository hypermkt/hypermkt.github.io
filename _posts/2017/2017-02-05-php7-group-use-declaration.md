---
layout: post
title: PHP7文法調査(1) use宣言のグループ化
post_id: 949
categories: 
- PHP
---

## はじめに



[PHP7.0.x](http://php.net/manual/ja/migration70.new-features.php)から追加された新機能、文法について把握できていないので、気になった箇所から調べてみようと思います。


## 概要



>複数のクラスや関数そして定数を同じ namespace からインポートする際に、 単一の use 文にまとめられるようになりました。


今までuseでクラスのインポートをする際は、必要な数だけuseを記述する必要がありましたが、同じ名前空間の場合は一行で記述出来るようになりました。この記法であれば重複も減り、可読性もあがっていいですね。


### BEFORE



use Sample\Hoge;
use Sample\Fuga;
use Sample\Bar;


### AFTER



use Sample\{Hoge, Fuga, Bar};


## 参考サイト



*  [PHP: 新機能 - Manual](http://php.net/manual/ja/migration70.new-features.php)


*  [PHP: rfc:group_use_declarations](https://wiki.php.net/rfc/group_use_declarations)