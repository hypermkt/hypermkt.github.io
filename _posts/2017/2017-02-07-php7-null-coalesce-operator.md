---
layout: post
title: PHP7文法調査(2) Null合体演算子
post_id: 967
categories: 
- PHP
---

## はじめに



[前回](https://hypermkt-blog.lolipop.io/php7-group-use-declaration/)の続き、PHP7の文法を見ていきます。今回は前々から気になっていた ?? です。


## 概要



[php.net](http://php.net/manual/ja/migration70.new-features.php)では以下のように説明されています。


>null 合体演算子 (??) がシンタックスシュガーとして追加されました。 三項演算子と isset() を組み合わせる よくありがちなパターンを、より簡単に書くためのものです。 この演算子は、もし第一オペランドが非 NULL の値であればそれを返し、 そうでない場合は第二オペランドを返します。


サンプルコードを書くと以下のようになります。


<?php

// 左辺がNULLでなければそれを返し、NULLならば右辺を返す。
echo $_GET['user'] ?? 'This is null';

// 以下と同じ意味
echo isset($_GET['user']) ? $_GET['user'] : 'This is null';

確かに今まではissetを利用してNULL判定する機会は多々ありましたが、冗長なコードになりがちでした。??を利用することで重複箇所が減り、コードもすっきりします。


## NULL合体演算子が生まれた経緯



[RFC](https://wiki.php.net/rfc/isset_ternary)を見ると面白いことが書いてあります。要約すると


*  PHPはWebに焦点をあてた言語なので、ユーザーデータの処理は頻繁に行われます。


*  isset($_GET['mykey']) ? $_GET['mykey'] : "" とチェックするのは面倒くさい


*  短縮版三項演算子 ?: は便利にしてくれますが $_GET['mykey'] ?: "" の場合にNoticeエラーが起きる（Undefined index)


*  これらの問題解決のため、?: の修正やifsetor()が頻繁に要求されてきた

結局何が原因でここまで議論されてきたのかと言うと、精査したい対象の値が$_GET['no_index']のような場合だと、Noticeエラーがあがってしまうのが問題とのことでした。それを回避する手段として?:の修正案や
[ifsetor](https://wiki.php.net/rfc/ifsetor)が提案されてきたんですね。

ちなみにifsetorの実装例は下記のようなコードだそうです。


function ifsetor($value, $default = null) {
    return isset($value) ? $value : $default;
}

確かにこれであればNoticeエラーも回避できますが、どうも名前が分かりづらい印象です。

そして今回満を持して ?? 演算子が実装されたようです。


## 終わりに


RFCも確認するとそれがどういった経緯で導入されたのか知れて面白いですね。関連する議論については2004年から始まっており、12年越しに生まれた文法が ??演算子なので、機会があれば積極的に使っていきたいですね。