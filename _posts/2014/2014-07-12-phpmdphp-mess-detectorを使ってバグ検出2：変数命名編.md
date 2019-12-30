---
layout: post
title: PHPMD(PHP Mess Detector)を使ってバグ検出2：変数命名編
post_id: 321
categories: 
- PHP
- phpmd
---

前回の
[](https://hypermkt-blog.lolipop.io/phpmdphp-mess-detector%e3%82%92%e4%bd%bf%e3%81%a3%e3%81%a6%e3%83%90%e3%82%b0%e6%a4%9c%e5%87%ba%ef%bc%9a%e6%9c%aa%e4%bd%bf%e7%94%a8%e5%a4%89%e6%95%b0%e7%b7%a8/)に続いて今回は変数命名について調べました。PHPMD実行時のオプションで
namingを渡すことで下記をチェックしてくれます### 短すぎる変数名



*  メンバ変数


*  ローカル変数


*  引数


*  最小値 : 3文字


### 長過ぎる変数名



*  メンバ変数


*  ローカル変数


*  引数


*  最大値 : 20文字


### 短すぎる関数名



### クラス名と同名の関数名の使用禁止



*  PHP4の時はクラス名と同名の関数名がコンストラクタとなりましたが、PHP5の場合コンストラクタは__constructになります。


### 定数の大文字チェック



*  定数は大文字で宣言すべきなので、小文字だと指摘されます。


### getメソッドなのにboolean型を返そうとしている



*  boolean型の値を返したい場合はisXかhasXを利用すべきなので、getでboolean型を返そうとする場合は指摘されます。


*  ただしこのチェックは実際の関数の中をみるのではなくコメント欄と@return booleanをみているので、コメントが記述されていない場合はスルーされます。


## 試してみる



<?php

class BadNaming {
    const bad_naming = 0;
    const GOOD_NAMING = 1;

    public $a;
    protected $toooooooooooooooooooooooooooooooLong;

    public function BadNaming() {
        // php4 style
    }

    public function a($d) {
        $e = 'Hello World';
        echo $e;
    }

    public function main() {

    }

    /**
     * Return a boolean
     *
     * @return boolean
     **/
    public function getFoo() {
        return true;
    }

    public function getBar() {
        return true;
    }
}

下記の通り注意されます。


$ ./vendor/bin/phpmd BadNaming.php text naming

BadNaming.php:4 Constant bad_naming should be defined in uppercase
BadNaming.php:7 Avoid variables with short names like $a. Configured minimum length is 3.
BadNaming.php:8 Avoid excessively long variable names like $toooooooooooooooooooooooooooooooLong. Keep variable name length under 20.
BadNaming.php:10    Classes should not have a constructor method with the same name as the class
BadNaming.php:14    Avoid variables with short names like $d. Configured minimum length is 3.
BadNaming.php:14    Avoid using short method names like BadNaming::a(). The configured minimum method name length is 3.
BadNaming.php:15    Avoid variables with short names like $e. Configured minimum length is 3.
BadNaming.php:28    The 'getFoo()' method which returns a boolean should be named 'is...()' or 'has...()'