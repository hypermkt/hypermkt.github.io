---
layout: post
title: PHP7文法調査(3) スカラー型宣言
post_id: 978
categories: 
- PHP
---

## はじめに


個人的には待ってました！というべき機能でした。三行でまとめると


*  引数の型宣言の種類が増えた


*  関数の戻り値の型も指定できるようになった


*  緩い型、強い型モードの２種類が設定できるようになった

となります。早速確認していきましょう。


## 新しい型宣言



[php.netの型宣言](http://php.net/manual/ja/functions.arguments.php#functions.arguments.type-declaration)によると、以下の4種類が新しい型として利用できるようになりました。


*  ブーリアン型 bool


*  文字列型 string


*  数値型 int


*  浮動小数点数型 float

PHP5.0はクラス、PHP5.1は配列、PHP5.4はcallableが指定できましたが、型宣言として不十分でした。PHP7からやっと本格的な型宣言ができるようになったという印象です。


## 戻り値の型宣言が出来るようになった


PHP7の大きな仕様追加の一つに上げられるのは、この
[戻り値の型宣言](http://php.net/manual/ja/migration70.new-features.php#migration70.new-features.return-type-declarations)だと思います。


### サンプルコード1


下記のように戻り値に文字列を指定した関数で配列を返そうとすると、


function hoge1(): string
{
  return array();
}

echo hoge1();


### 実行結果


下記エラーが発生するようになり、より関数の入出力が厳密な実装ができるようになりました。


PHP Fatal error:  Uncaught TypeError: Return value of hoge1() must be of the type string, array returned in


## デフォルトでは弱い型検査、設定で強い型検査も可能



[強い型付け](http://php.net/manual/ja/functions.arguments.php#functions.arguments.type-declaration.strict)のページによると、以下となります。


*  弱い型、強い型の２種類があり、デフォルトでは弱い型が選択されている


*  弱い型の場合に、間違った型が渡されても可能な限り型の自動変換が適用される


*  厳密な型宣言にするには、declare文でstrict_types = 1を宣言することにより適用できる

少し分かりにくいので、サンプルコードを交えながら見ていきましょう。

1番はそういうものとして、2番が一番気になりました。例えば以下のようなコードです。


### 型の自動変換



サンプルコード2


function hoge1(string $hoge)
{
  var_dump($hoge);
}

hoge1('hoge'); // OK
hoge1(1); // OK


実行結果


string(4) "hoge"
string(1) "1"

実行結果を見ると面白いことが起きています。数値の1が文字列の1に変換されているのが分かります。これが型の自動変換の結果のようですね。


### 厳密な型宣言



declare(strict_types=1);を宣言すると、サンプルコード２の実行結果と変わりました。stringの時は、stringしか通らなくなりました。


サンプルコード3


declare(strict_types=1); // 厳密な型検査モードにする

function hoge1(string $hoge)
{
  return $hoge;
}

echo hoge1('hoge'); // OK
echo hoge1(1); // NG


実行結果


hogePHP Fatal error:  Uncaught TypeError: Argument 1 passed to hoge1() must be of the type string, integer given, called


## ２つの型検査が生み出す新たな世界


この２つの型検査の仕様を知った時、どうしてどちらかにならなかったんだろうか？と単純に思いました。その経緯と回答については、
[東北ギーク ～仙台にあるWeb系会社システム部のブログ～さん](http://tech.respect-pal.jp/)の
[3 導入に至るまでの経緯](http://tech.respect-pal.jp/php7_scalar_type_hinting/#i-5)の記事に詳しくまとまってるので、そちらをご覧ください。

要するに、弱い型検査、強い型検査、双方にメリット・デメリットがありますが、議論の末に両方の導入が決まり選択はエンジニアに委ねられたということでした。

個人的にはどちらかにしてほしかったと思います。選択肢があるということは、導入時にはそれを決めるコスト、利用時にはそれを確認するコストの両方が発生します。がっつりとPHP7のコードの世界には入っていないので、どんなことは待ち受けているか分かりませんが、今後どちらが良いのか見極めていきたいです。まずは弱い型検査を使ってみようと思います。


## 終わりに


PHP7文法チェックを調べれば調べるほど、新仕様には長年の議論の末に導入されたという事実が潜んでいることが多いことが分かりました。特に今回のスカラー型宣言は、その典型例でした。文法を知ることも大切ですが、こういった背景を知った上で利用するのも面白いと思いました。


## 参考サイト
*  [PHP: 新機能 - Manual](http://php.net/manual/ja/migration70.new-features.php#migration70.new-features.scalar-type-declarations)
*  [PHPの型と型安全について(PHP7からのPHPプログラミング) - Qiita](http://qiita.com/bokotomo/items/1ead446ef689dc45b487)
*  [【導入決定！】PHP7で実装されるスカラー型宣言とは？ | 東北ギーク](http://tech.respect-pal.jp/php7_scalar_type_hinting/)