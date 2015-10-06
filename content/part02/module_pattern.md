### モジュールパターンについて知る

モジュールパターンは、
- 名前空間
- プライベートメンバとパブリックメンバの使い分け
- 即時関数(3-4で説明)

などを組み合わせて作る、コード作成のためのパターンです。モジュールパターンを適用することによって、機能別にコードを分割することができます。さらに、各機能ごとにコードをまとめることによって、機能の拡張・削除・リファクタリングが容易になります。また、導入も簡単なので、結構広く使われていると思います。

以下の順で説明していきます。

- モジュールパターン(1)：名前空間の準備
- モジュールパターン(2)：モジュールの定義
- モジュールパターン(3)：モジュールを使う
- モジュールパターン(4)：モジュールの公開


#### モジュールパターン(1)：名前空間の準備

まずは名前空間を準備します。

```js

var MYAPP = MYAPP || {
  util: {
    math: {}
  },
  data: {
    int: {}
  }
};

```

#### モジュールパターン(2)：モジュールの定義

次に、モジュールを定義します。

ここで、プライバシーが必要なときに、即時関数が活用できます。

```js

MYAPP.util.math = (function(){
  // ここに処理を書いていきます。
}());

```

たとえば、ここでは加減乗除のメソッドを提供しているとしましょう。

```js

MYAPP.util.math = (function(){
  return {
    add: function(x, y){
      return x + y;
    },
    minus: function(x, y){
      // xよりyが大きい時は「x-y」を、そうでない場合は「y-x」を返します
      return x > y ? x - y : y - x;
    },
    multiply: function(x, y){
      return x * y;
    },
    divide: function(x, y){
      // yが0のとき「NaN(Not a number)」を返します
      return y !== 0 ? x / y : "NaN";
    }
  };
}());

```

このように、即時関数が提供するプライベートスコープを使うことで、
プライベートのプロパティとメソッドを必要に応じて宣言することができます。

#### モジュールパターン(3)：モジュールを使う

最後に、この即時関数が返すオブジェクトを指定します。
(1),(2)のコードをまとめてかきます。

```js

// (1)名前空間の準備
var MYAPP = MYAPP || {
  util: {
    math: {}
  },
  data: {
    int: {}
  }
};

// (2)モジュールの定義
MYAPP.util.math = (function(){


  return {
    add: function(x, y){
      return x + y;
    },
    minus: function(x, y){
      // xよりyが大きい時は「x-y」を、そうでない場合は「y-x」を返します
      return x > y ? x - y : y - x;
    },
    multiply: function(x, y){
      return x * y;
    },
    divide: function(x, y){
      // yが0のとき「NaN(Not a number)」を返します
      return y !== 0 ? x / y : "NaN";
    }
  };
}());

// (3)モジュールを使う
MYAPP.util.math.add(4,5);
MYAPP.util.math.minus(4,5);
MYAPP.util.math.multiply(4,5);
MYAPP.util.math.divide(4,5);

```

#### モジュールパターン(4)：モジュールの公開

それでは、今まで書いたコードを
- まず`add`,`minus`などのメソッドをプライベートにする
- 続いて、パブリックAPIを開示する（返すメソッドを指定する）

ことで、簡便なモジュールパターンの例の紹介を締めくくりたいと思います。


```js

// (1)名前空間の準備
var MYAPP = MYAPP || {
  util: {
    math: {}
  },
  data: {
    int: {}
  }
};

// (2)モジュールの定義
MYAPP.util.math = (function(){

  // これらメソッドをプライベートメンバとして定義
  add = function(x, y){
    return x + y;
  },
  minus = function(x, y){
    // xよりyが大きい時は「x-y」を、そうでない場合は「y-x」を返します
    return x > y ? x - y : y - x;
  },
  multiply = function(x, y){
    return x * y;
  },
  divide = function(x, y){
    // yが0のとき「NaN(Not a number)」を返します
    return y !== 0 ? x / y : "NaN";
  }

  // public API
  return {
    add: add,
    minus: minus,
    multiply: multiply,
    divide: divide
  };
}());

// (3)モジュールを使う
MYAPP.util.math.add(4,5);
MYAPP.util.math.minus(4,5);
MYAPP.util.math.multiply(4,5);
MYAPP.util.math.divide(4,5);

```

以上でモジュールパターンの説明を終わりたいと思います。
