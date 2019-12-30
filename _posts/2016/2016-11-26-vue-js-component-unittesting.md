---
layout: post
title: Vue.js componentでvue-router,vue-resourceを利用したメソッドのUnitTestを書く方法
post_id: 873
categories: 
- Vue.js
---

## 始めに


最近vue-resource・vue-routerを使ったSPAウェブサービスを開発・運用しており、ふとそろそろUnit Testを書いてみたいな〜と思い、ググってみたのですが思っていた以上に情報が見つからず・・・。ネット上で見つけたのは下記の３つのみ。


*  [公式サイトのUnit Testing](https://vuejs.org/v2/guide/unit-testing.html)


*  [Vuejs testingのスライド](http://www.slideshare.net/coulix/vuejs-testing?qid=cf224629-58db-4968-bc7b-6d3dfab85d58&v=&b=&from_search=1)


*  [Rails + Vue.jsなアプリケーションではじめてのフロントエンドのユニットテスト - Qiita](http://qiita.com/edwardkenfox/items/a44dd04fb80c30c53aba)

公式サイトを見ながら見よう見まねで書いてみたが、ハマりにハマってしまいもう無理だーとVue.jsコミュニティの方々に助けを求めました。皆さんから多数の提案を頂き、無事に解決しました。本当にありがとうございます！

今回はVue.jsコミュニティの方々に教えて頂いた、Vue.js componentのUnitTestを書く方法をまとめます。


## 前提


下記ライブラリを使用します。


*  Vue.js 2.0 + vue-resource + vue-router + vueify


*  karma


*  mocha


*  chai


## テストケース ３パターン



*  普通のコンポーネントメソッド


*  vue-routerのパラメーターを利用したメソッドのテスト


*  vue-resourceでAPI呼び出しをしているメソッドのテスト


### テストについて



*  Vue.jsの
*  .vueのコンポーネントを対象とし、
methods内のメソッドをテストします。


*  一部省略していますが、
[こちら](https://github.com/hypermkt/vuejs-samples/tree/master/vue-component-unittest)にサンプルコードが置いてありますのでこちらをご覧ください。


### パターン１：普通のコンポーネントメソッド


まずは外部ライブラリ、リソースを全く利用しない一番シンプルな例です。下記のような
addという足し算メソッドがあったとします。


export default {
  methods: {
    add(a, b) {
      return a + b;
    }
  }
}

上記のメソッドのテストをする場合は以下のように記述します。ポイントは以下の通りです。
*   テスト対象のコンポーネントをimportします。（読み込み時にファイルパスは適度の調整してください。）
*   コンポーネント内のメソッドへはmethodsを通じて呼び出すことができます。これはVueの基本仕様通りメソッドはmethods内にあるからです。


import Test from '../../../src/js/components/test.vue';

describe('Testコンポーネント', () => {
  it('足し算', () => {
    expect(Test.methods.add(1, 2)).to.be.eql(3);
  })
});

また下記のようにすればメソッドの存在確認テストもできます。


import Test from '../../../src/js/components/test.vue';

describe('Testコンポーネント', () => {
  it('足し算', () => {
    expect(Test.methods.add).to.be.a('function');
  })
});

ここまでは自分で出来ました。


### パターン２：vue-routerのパラメーターを利用したメソッドのテスト


ここからは自分では解決できず、コミュニティの方の助言で解決しました。例えば下記のように vue-router のパラメーターを取得して文字列結合をするとします。


export default {
  methods: {
    myName() {
      return this.$route.params.name + ' Taro';
    }
  }
}

上記のメソッドのテストをする場合は以下のように記述します。ポイントは以下の通りです。


*  [Vue.extend](https://jp.vuejs.org/v2/api/#Vue-extend)でサブクラスを作成し、テストケースの中でインスタンス化します


*  またインスタンス化時に
[beforeCreate](https://jp.vuejs.org/v2/api/#beforeCreate)でルーティングのパラメーターを設定することが出来、ルーティングされた体でテストを書くことが出来るようになります。


import Vue from 'vue';
import _Test from '../../../src/js/components/test.vue';

const Test = Vue.extend(_Test);

describe('Testコンポーネント', () => {
  it('vue-routerを利用した場合のテストケース', function() {
    const vm = new Test({
      beforeCreate() {
        this.$route = { params: { name: 'Yamada' } }
      }
    });
    expect(vm.myName()).to.be.eql('Yamada Taro')
  });
});


### パターン３：vue-resourceでAPI呼び出しをしているメソッドのテスト


今回一番悩んだのがこのテストケースでした。APIを利用してJSONで値を取得し、success時にデータオブジェクトのprofileを変更したい場合です。API呼び出しには 
[vue-resource](https://github.com/pagekit/vue-resource)のグローバル関数 Vue.http.get を利用していました。メソッドの返り値はPromiseとなり、非同期処理のテストってどうやるのか・・・。


export default {
  data() {
    return {
      profile: {}
    }
  },
  methods: {
    fetchProfile() {
      Vue.http.get('http://localhost:8000/api/profile').then((response) => {
        console.log("success");
        this.profile = response.json();
      }, (response) => {
        console.log("failure");
      });
    }
  }
}

ゴールのイメージとしては外部リソースであるAPIを実行しないようにモック化し、テスト対象のメソッド内で置き換えをすることです。

今回は Vue.http.getとグローバル関数を呼ぶので、下記のように
[算出プロパティ](https://jp.vuejs.org/v2/guide/computed.html)を利用して
$Vueからテスト対象オブジェク自身を取得できるように調整します。


export default {
  data() {
    return {
      profile: {}
    }
  },
  computed: {
    $Vue() { return Vue; }
  },
  methods: {
    fetchProfile() {
      Vue.http.get('http://localhost:8000/api/profile').then((response) => {
        console.log("success");
        this.profile = response.json();
      }, (response) => {
        console.log("failure");
      });
    }
  }
}

次にテストケースです。ポイントは以下の通りです。


*  まずテストケースの引数にdoneを指定します。
[mochaの仕様](https://mochajs.org/#asynchronous-code)でdone()の実行をもって非同期テストが終了したことをmochaに通知します。（通例的に
doneと命名されるそうです）


*  Vue.http.getでAPIを実際に呼び出さないようにモック化。getメソッドでPromiseの成功時にjson()メソッドがprofileのオブジェクトを返すように設定します。sinon等のモックライブラリを使わず、今回は仮の値を返す方式でも問題ありません。そしてテストケース2と同じ方式でコンポーネントをインスタンス化し、今回のテストケース用に設定した
$Vueを通じて、Vueインスタンスのグローバル変数を上書きします。


*  最後にテスト対象のメソッドを実行します。今回は非同期のテストにつき、そのままではPromiseの成功処理に入らないので、setTimeoutで擬似的に非同期にします。setTimeout内のdone()の実行をもって非同期テストが終了したことをmochaに通知され、モックで指定した通り 
Promise.resolveで成功処理に入りテストケースが通ります。


import Vue from 'vue';
import _Test from '../../../src/js/components/test.vue';

const Test = Vue.extend(_Test);

describe('Testコンポーネント', () => {
  // ステップ１：引数のdoneを指定  
  it('vue-resourceを利用した場合', (done) => {
    let profile = { nickname : 'hoge' };

    // ステップ２：Vue.http.getをモック化
    const vm = new Test()
    vm.$Vue.http = {
      get () {
        return Promise.resolve({
          json () { return profile }
        });
      }
    }
    
    // ステップ３：setTimeoutで非同期呼び出し
    vm.fetchProfile();
    setTimeout(() => {
      expect(vm.profile).to.be.eql(profile)
      
      // mochaの非同期用テスト仕様で、done()が実行されるまでテストは実行されない。
      done();
    }, 0)
  });
});


## Special Thanks


今回はVue.jsコミュニティの下記の方々に１週間に渡って何度も助言頂きました。最後にはパッチまで準備して頂きありがとうございます！とても勉強になりました。


*  [@chi-bd](https://github.com/chi-bd)


*  [@kitak](https://twitter.com/kitak)


*  [@ktsn](https://twitter.com/ktsn)


## 感想


普段からJavaScriptのテストコードを全く書いたことがなかったので、正直基本的な事から理解が出来ず苦労しました。ただVue.jsを通じてテストを書く機会が生まれて、この３パターンなら書けるようになりました。JavaScriptのテストコードコード楽しいですね〜。もっと書きたくなったぞー！