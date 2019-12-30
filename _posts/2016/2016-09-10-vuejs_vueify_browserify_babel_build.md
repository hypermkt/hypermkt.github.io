---
layout: post
title: Vue.js + vueify + browserify + Babel でビルドする方法
post_id: 824
categories: 
- Vue.js
- vueify
---

## 概要


vueify利用時に
babel-coreをインストールするので、
*  .vueファイル内はES2015形式でも記述できるのですが、それ以外のファイルはES5形式で書く必要があります。一部だけ記法が違うのも気持ち悪かったので、他のJSファイル内でもES2015形式で書けるようにする方法をまとめます。


## 用語説明



### ES2015


ES2015とはJavaScriptの仕様バージョンのことで極端に言えばPHP5などと同じようなものです。現行のほとんどのブラウザはES5と呼ばれるバージョンになっていますが、ES2015から新しい文法が使えるようになります。別名ES6とも呼ばれています。（この辺が紛らわしい）


### vueify


Vue.jsのコンポーネントを 
*  .vue ファイルに分割することができます。また一つの 
.vue ファイル内でcss/template/scriptの３つを管理することができ見通しが良くなります。


### browserify


ブラウザ動作用に記述したJavaScriptコード内で 
require('modules') が使えるようになります。


### Babel


ES2015に対応したトランスコンパイラです。ES2015形式で書いたJavaScriptコードをES5に変換してくれます。


## 参考ソースコード


package.json


{
  "scripts": {
    "build": "browserify -e main.js -o app.js"
  },
  "browserify": {
    "transform": [
      "vueify",
      "babelify"
      ]
  },
  "devDependencies": {
    "babel-core": "^6.14.0",
    "babel-plugin-transform-runtime": "^6.15.0",
    "babel-preset-es2015": "^6.14.0",
    "babel-runtime": "^6.11.6",
    "babelify": "^7.3.0",
    "browserify": "^13.1.0",
    "vue": "^1.0.26",
    "vueify": "^8.7.0"
  }
}

 

index.html


<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title></title>
</head>
<body>
<div id="app">
  <common_header></common_header>
  <hr />
  {{ message }}
  <hr />
  <common_footer></common_footer>
</div>
<script src="app.js"></script>
</body>
</html>

main.js


import Vue from 'vue';
import Header from './components/header.vue';
import Footer from './components/footer.vue';

new Vue({
  el: '#app',
  components: {
    // header, footerは予約後のため使えない
    common_header: Header,
    common_footer: Footer,
  },
  data: {
    message: 'Hello Vue.js'
  }
});

components/heaeder.vue


<template>
header
</template>

components/footer.vue


<template>
footer
</template>


## 設定方法



### 必要パッケージのインストール


まずは Vue.js + vueify + browserify + Babel でビルドするために必要なパッケージを全てインストールします。


npm install\
    vue\
    vueify\
    babel-core\
    babel-present-es2015\
    babel-runtime\
    babel-plugin-transform-runtime\
    browserify\
    baberify\
    --save-dev


babel-core〜babel-runtime

これらのパッケージは vueify インストール時の必要パッケージとして要求されます。これらをインストールすることで、 
*  .vue 内ではES2015でコードを書くことができます。


babelify

browserify でBabel変換するためのパッケージです。


### browserify.transform項目


refs: https://github.com/substack/browserify-handbook#browserifytransform-field

package.json内でbrowserify.transform項目を記述することで、browserifyが実行された際に自動的にtransform設定を適用することができます。今回の場合ですと、 
vueify と 
babelily の２つを同時に適用したので下記のように記述します。


"browserify": {
    "transform": [
      "vueify",
      "babelify"
     ]
  },


### .babelrc



{
  "presets": ["es2015"],
  "plugins": [
    "transform-runtime"
  ]
}


presets

babelで変換するバージョンを指定します


transform-runtime


babel-plugin-transform-runtime をインストールすると利用できるようになるbabel用プラグイン。似たようなものに 
babel-polyfill があるらしいがグローバル汚染するのことでこちらを利用するのがいいらしい。babel変換するのに必要。

参考 : 
[babel-polyfillとbabel-runtimeの使い分けに迷ったので調べた - Qiita](http://qiita.com/inuscript/items/d2a9d5d4daedaacff924)


## ビルド



package.json の 
scripts で 
build を宣言しているので下記で実行できます。


npm run build

結果、下記のように表示されれば成功です。


![vuejs_babelify](/images/vuejs_babelify.png)

本件のソースコードは
[こちら](https://github.com/hypermkt/vuejs-samples/tree/master/vueify-babel-build)に置いてあります。


## 感想


正直随分ハマりましたが、これでES2015でコードが書けるようになって幸せです。（といいつつも全く書けません。勉強中です。）