### 名前空間のイロハ

名前空間は、中規模以上のアプリを書くにあたって必須のパターンです。
JavaScriptでは、グローバルスコープを汚染することを防ぐ代わりに、
唯一のアプリケーションのグローバルオブジェクトを作成することが主流です。

また、プロトタイプチェーンを用いることによって、
モジュールコンテナを作成することもできます。

```js

var MYAPP = {};

MYAPP.name = "My First SPA";
MYAPP.data = "2015/06/25";

MYAPP.Update = function(){
  ...
};
MYAPP.Delete = function(){
  ...
};

// オブジェクトのコンテナ
MYAPP.modules = {};

MYAPP.modules.name = "My First SPA's first module";
MYAPP.modules.data = {
  1 : "test",
  2 : "test2",
  3 : "test3"
};

```

#### ベストプラクティス：名前空間

なお、名前空間を作成する際には、サードパーティプラグインの読み込みや
逆にプラグインとして公開して使ってもらう場合に、この唯一のグローバルオブジェクトが
競合してしまう可能性がないとも言い切れません。

したがって、以下のような定義をします。

```js

if (typeof MYAPP === "undefined"){
  var MYAPP = {};
}

```

そして、これをもっと短くした以下の書き方もあります。

```js

var MYAPP = MYAPP || {};
var MYAPP.module = MYAPP.module || {};

```

#### ベストプラクティス：『JavaScript パターン』における名前空間

JavaScriptが提供している名前空間のメソッドなどはないため、
おのおのが自分の最善と思える名前空間の定義をしており、
先人の方々の例が「JavaScript 名前空間」などとググるとたくさん
参考にできます。

<a href="http://www.amazon.co.jp/gp/product/4873114888/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=4873114888&linkCode=as2&tag=kenjuw-22">『JavaScriptパターン』(O'Reilly)</a><img src="http://ir-jp.amazon-adsystem.com/e/ir?t=kenjuw-22&l=as2&o=9&a=4873114888" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
のp91-92にある`namespace()`メソッドは非常に示唆に富んでいるので、ここで紹介
したいとおもいます。作者のStoyan Stefanovさんの説明は非常にわかりやすいので、ここでわからなかったらぜひ原著を見てみてください。

まず、いかのように名前空間を準備したいとします。

```js

var MYAPP = {
  modules: {
    moduleA: {},
    moduleB: {}
  }
};

```

これを、たとえば以下のような`namespace()`メソッドで定義できるようにしましょう。

```js

MYAPP.namespace('MYAPP.modules.moduleA');
MYAPP.namespace('MYAPP.modules.moduleB');

```

また、ここで紹介されているメソッドは、以下のような宣言方法にも対応しています。

```js

// 戻り値をローカル変数に代入する
var module2 = MYAPP.namespace('MYAPP.modules.module2');
module2 === MYAPP.modules.module2; // true

// 先頭のMYAPPを省略
MYAPP.namespace('modules.module51');

// 長い名前空間
MYAPP.namespace('once.upon.a.time.there.was.this.long.nested.property');

```

では、実装の中身を見ていきましょう。

```js

var MYAPP = MYAPP || {};

MYAPP.namespace = function(ns_string){
  var parts = ns_string.split('.'), // . で区切った配列
      parent = MYAPP, // グローバルオブジェクトのアプリ名
      i;

  // 先頭のグローバルを取り除く
  if ( parts[0] === "MYAPP"){
    parts = parts.slice(1); // 先頭を削除
  }

  for ( i = 0; i < parts.length; i += 1){
    // プロパティが存在しなければ作成する
    if ( typeof parent[parts[i]] === "undefined"){
      parent[parts[i]] = {}; // モジュールのオブジェクト生成
    }
    parent = parent[parts[i]];
  }
};

```