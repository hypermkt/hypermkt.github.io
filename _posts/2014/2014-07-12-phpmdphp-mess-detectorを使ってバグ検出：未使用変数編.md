---
layout: post
title: PHPMD(PHP Mess Detector)を使ってバグ検出：未使用変数編
post_id: 313
categories: 
- PHP
---

バグがない世界に行けるものなら行きたいけど、そんな世界は残念ながら存在しないです。
ただバグを少なくすることは可能で今日はつまらないバグを未然に防ぐことが出来るツール
[PHP Mess Detector](http://phpmd.org/)を見つけたので早速使ってみたのでまとめてみる。##PHP Messdetectorとは？


指定のソース、ディレクトリ内のPHPソースコードを解析し、バグの可能性になりうる箇所を検知しリストアップしてくれます。
*   命名ルール
*   未使用変数、関数
などなど


## インストール


下記２つを追加してcomposer installするだけでOK。注意点としては
pdepend/pdependに依存してくるので追記を忘れないこと。


{
  "require": {
    "phpmd/phpmd" : "1.4.*",
    "pdepend/pdepend" : "1.1.0"
  }
}


## 使い方


下記形式に手動実行するだけで可能。


./vendor/bin/phpmd #{対象ファイル名} #{結果の出力形式} #{チェック項目の種類}


## 使ってみる


下記のサンプルソースコードを使って一番分かりやすい未使用変数チェックをしてみる。


<?php

class Hoge
{
    public $_publicUnused;
    protected $_protectedUnused;
    private $_privateUnused;

    public function pubicUnusedFunction()
    {
        $localUnused = 'Hello';
        $localUsed = 'World';
        echo $localUsed;
    }

    protected function protectedUnusedFunction($unused)
    {
    }

    private function privateUnusedFunction($used = 'howdy')
    {
        echo $used;
    }
}

実行してみたら見事に検知できた。テストで自分のプロジェクトのソースファイルに対して実行してみると未使用変数がいくつか見つかりぞっとした・・・。明らかに無駄なデータ取得してるよね・・・。


$ ./vendor/bin/phpmd Hoge.php text unusedcode

Hoge.php:7  Avoid unused private fields such as '$_privateUnused'.
Hoge.php:11 Avoid unused local variables such as '$localUnused'.
Hoge.php:16 Avoid unused parameters such as '$unused'.
Hoge.php:20 Avoid unused private methods such as 'privateUnusedFunction'.


unusedcodeは下記の４項目をチェックしてくれる。この項目はどれもよくあるあるでとても分かりやすいし修正も簡単な例ですね。その反面コードの量が増えるとその中にまぎれてしまい見つかりにくくなるのでこういうのこそ機械的にPHPMDで検知してしまうのがいいですね。
refs : http://phpmd.org/rules/unusedcode.html


### 未使用privateメソッド



*  未使用のprivateメソッドを検出する。public/protecedは検出されない。


### 未使用privateメンバ変数



*  未使用のprivateメンバ変数を検出する。public/protecedは検出されない。


### 未使用ローカル変数



### 未使用引数


今回はPHPMDでunusedcodeオプションを使い未使用変数を検知してみたが他にもオプションがあるので調べてみよう。