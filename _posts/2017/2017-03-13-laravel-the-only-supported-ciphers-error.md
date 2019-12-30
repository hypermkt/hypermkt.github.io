---
layout: post
title: The only supported ciphers are AES-128-CBC and AES-256-CBCエラーにハマった
post_id: 996
categories: 
- Laravel
- PHP
---

## 概要

Laravelで開発したアプリケーションを閲覧した際に「
The only supported ciphers are AES-128-CBC and AES-256-CBC」というエラーが表示され結構ハマってしまいました。本エラーの原因と解決方法をまとめます。

## 前提


*  Laravel 5.4

## 原因


*  このエラー文言は 
[Encrypterクラス](https://github.com/laravel/framework/blob/5.4/src/Illuminate/Encryption/Encrypter.php#L43)から例外で送出されます。
supportedメソッドを見ると暗号化名と桁数が一致しないとfalseとなりこの問題が発生します。

 	
*  つまり
.envファイル内で指定した
APP_KEYと
config/app.phpの
cipherで指定した暗号と桁数が一致していないのが原因です。

## 解決方法

確認すべきは以下です。

*   
*  *.env**
内で
*  *APP_KEY**
が指定されているか。指定されていなければ、
*  *php artisan key:generate**
を実行する

 	
*   
*  *APP_KEY**
を復号化して桁数を調べる

 	
*   もし16桁なら
*  *AES-128-CBC**
を、32桁なら
*  *AES-256-CBC**
を指定する
これで解決します。

## 参考


*  [The only supported ciphers are AES-128-CBC and AES-256-CBC](https://github.com/laravel/framework/issues/9080)