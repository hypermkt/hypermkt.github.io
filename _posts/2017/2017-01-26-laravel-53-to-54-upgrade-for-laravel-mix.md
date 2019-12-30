---
layout: post
title: Laravel 5.3から5.4アップグレード：Laravel Mix編
post_id: 936
categories: 
- Laravel
- PHP
---

### はじめに



[Laravel 5.4が２日前にリリース](https://laravel-news.com/category/laravel-5.4)されていたことを知り、5.3で開発を進めていたアプリケーションを5.4へのアップグレードを試みました。
[Upgrade Guide](https://laravel.com/docs/5.4/upgrade)を見ると、Laravel ElixirからLaravel Mixへの乗り換え方法が記述されておらず、途方に暮れていた所、Laravelリファレンス著者
[@localdiskさん](https://twitter.com/localdisk)から即レスで手順を教えていだきました。ありがとうございます。


>[@hypermkt](https://twitter.com/hypermkt) packages.json から laravel-elixir をリリースして laravel-mix を足す。node_modules/laravel-mix/setup/webpack.mix.js をコピーすればOK— MATSUO Masaru (@localdisk) 
[2017年1月25日](https://twitter.com/localdisk/status/824262581476921344)





まとめると以下となります。


### Laravel ElixirからLaravel Mixへの乗り換え手順



1. package.json内の 
laravel-elixir関連を削除して、
laravel-mix を追加


"laravel-elixir": "^6.0.0-14",
"laravel-elixir-vue-2": "^0.2.0",
"laravel-elixir-webpack-official": "^1.0.2",

を以下に変更します。
laravel-mix のバージョンは
[本家](https://github.com/JeffreyWay/laravel-mix)のリリースを見たところ、
v0.5.3 が最新でしたのでそちらを指定しました。


"laravel-mix": "^0.5.3",


2. 
laravel/laravelリポジトリから 
package.jsonの
scriptsセクションをコピー

ビルドツールがGulpからWebpackに変更となるためです。


"scripts": {
"dev": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
"watch": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
"hot": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
"production": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
},


3. コアコンポーネントのコピー

Laravel Elixirの 
gulpfile.js に当たるファイルです。


$ cp node_modules/laravel-mix/setup/webpack.mix.js .

webpack.config.js を編集したい場合は node_modules/laravel-mix/setup からコピーします。


$ cp node_modules/laravel-mix/setup/webpack.config.js .


--config のファイルパスを調整すれば完了です。
[マニュアル](https://github.com/JeffreyWay/laravel-mix/blob/master/docs/installation.md)によると 
webpack.config.js の配備は必須ではなく上級者用とのことなので、通常利用の場合は
webpack.mix.js だけコピーすれば問題無さそうです。


"scripts": { "dev": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=webpack.config.js", "watch": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=webpack.config.js", "hot": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=webpack.config.js", "production": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=webpack.config.js" },


4. パッケージインストール

まだ 
[yarnではインストールできない](https://github.com/JeffreyWay/laravel-mix/issues/82) とのことなので 
npm install します。


$ npm install

以上です。


### 動作検証


ビルド用の対象ファイルがあることが前提ですが、 
npm run dev を実行すると成功と表示されます。


DONE Compiled successfully in 1649ms

Asset Size Chunks Chunk Names
dist/app.js 2.75 kB 0 [emitted] app
dist/app.css 0 bytes 0 [emitted] app
mix-manifest.json 68 bytes [emitted]


### 終わりに


5.4もリリース直後ということで、もしかしたら同じように悩まれている方がいるかもしれないので、
[@localdiskさん](https://twitter.com/localdisk)に教えて頂いた内容そのままですがブロク記事にまとめさせて頂きました。参考になれば幸いです。


### 参考



*  [Upgrade Guide - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/5.4/upgrade)


*  [laravel-mix/installation.md at master · JeffreyWay/laravel-mix](https://github.com/JeffreyWay/laravel-mix/blob/master/docs/installation.md)