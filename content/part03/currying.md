
### カリー化

JavaScriptには、「部分適用」や「カリー化」といった考え方が存在します。
ココらへんは定義がわかりづらかったり、筆者自身も完璧に理解できているとは言いがたい部分も
ありますので、ざっくりと全体像でもお伝えできればと思います。

関数の適用、部分適用、カリー化の順番でお話していきます。

#### 関数の適用とは

そもそも、関数プログラミング言語においては、関数は「呼び出されるもの」というより、「適用されるもの」と捉えるほうが正確です。JavaScriptには`Function.prototype.apply()`メソッドがあります。

つまり、関数の呼び出しは`apply()`メソッドの糖衣構文（シンタックスシュガー）である、ということです。実際に`apply()`を使ってみます。

一つ目の引数には、関数の内部でthisに束縛されるオブジェクト、
二つ目の引数には、引数の配列で、関数の内部で使用したい値の配列を渡します。

```js

var multiply = function (x, y){
	return x * y;
};

multiply.apply(null, [4, 5]); // -> 20


```

一つ目の引数が異なる場合を紹介します。
名前空間`myApp`を用意し、その中の`multiply()`を適用します。

```js

var myApp = {
	add: function(x, y){
		return x + y;
	},

	multiply: function(x, y){
		return x * y;
	}
};

// 普通に呼び出す
myApp.add(4, 5);
myApp.multiply(4, 5);

// apply()で適用する
myApp.add.apply(myApp, [4, 5]);
myApp.multiply.apply(myApp, [4, 5]);

```

ここまでで、 **「関数は実は呼び出すものではなく適用されるもの」** ということがわかったでしょうか？次のフェーズに進みます。

#### 部分適用とは

関数の呼び出しには、実際には **「関数に引数の配列を適用する」** ことがわかりました。これがわかれば次は簡単です。「部分適用」とは、単純に、 **「引数を全てではなく一部だけ渡す」** ことを指します。

改めて、２つの引数をとる`multiply()`メソッドについて考えてみます。以下の例では、完全適用、部分適用の二パターンを紹介します。

```js

// 完全適用
var multiply = function(x, y){
	return x * y;
};

multiply.apply(null, [4, 5]);

// 部分適用
var partialMultiply = function(y){
	return multiply(4, y);
}

partialMultiply(5); // -> 20

```

部分適用では、第一引数を固定しています。
部分適用によって、`partialMultiply()`という新しい別の関数が得られました。
そしてその関数は別の引数で呼ぶことができます。

#### カリー化とは

カリー化された関数に引数を渡すだけで、別の関数を簡単に定義できる。それがカリー化の醍醐味です。

部分適用とカリー化の違いは、
- カリー化: 関数を引数１つずつに分割して適用させる
- 部分適用: 一部の引数を固定して新しい関数を作り出すこと

です。もっというと、
**「関数に部分適用を理解させ処理させること」** です。

先程の例では、以下のように`multiply()`メソッドの第一引数を「4」に固定していました。これは部分適用です。しかし、これでは汎用性が足りません。

```js

// 部分適用
var partialMultiply = function(y){
	return multiply(4, y);
}

partialMultiply(5); // -> 20

```

これをカリー化で表現すると、以下のようになります。

```js

function multiply(x, y){
	if(typeof y === "undefined"){ //部分適用
		return function(y){
			return x * y;
		};
	}
	//完全適用
	return x * y;
}

multiply(4)(5);

```

カリー化の特徴は、関数を呼び出す際に、引数をひとつずつ渡すようになっている
点です。これによって、「複数の引数を必要とする関数を、1つの引数の関数の定義だけで
同じ機能をもつ関数」に書き換えることが出来ました。

次に、引数が2つ以上の場合でも汎用的に使えるよう、`multiply()`を
書き換えてみましょう。

```js

function createCurry(func){
	var slice = Array.prototype.slice,
		stored_args = slice.call(arguments, 1);

	return function(){
		var new_args = slice.call(arguments);
		args = stored_args.concat(new_args);
		return func.apply(null, args);
	};
}

```

出典: <a href="http://www.amazon.co.jp/gp/product/4873114888/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=4873114888&linkCode=as2&tag=kenjuw-22">『JavaScriptパターン』</a><img src="http://ir-jp.amazon-adsystem.com/e/ir?t=kenjuw-22&l=as2&o=9&a=4873114888" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


このカリー化を使って、テストをしてみましょう。

```js

function multiply(x, y){
	return x * y;
}

// pattern A
var newMultiply = createCurry(multiply, 5);
newMultiply(4);

// pattern B
createCurry(multiply, 5)(4);

```

また、変換関数`createCurry()`のパラメータは複数でも使用可能です。
カリー化も複数段階で使うことができます。

```js

function calcVolume(x, y, z){
	return x * y * z;
}

// 複数の引数
createCurry(calcVolume, 5)(2, 3); // 30
createCurry(calcVolume, 5, 2)(3); // 30

// 2段階のカリー化
var calcVolumeWithWidthFive = createCurry(calcVolume, 5);
calcVolumeWithWidthFive( 2, 3 );
var calcVolumeWithWidthFiveAndHeightTwo = createCurry(calcVolumeWithWidthFive, 2);
calcVolumeWithWidthFiveAndHeightTwo( 3 );

```

#### 参考リンク

特に一つ目の、KDKTNさんの[食べられないほうのカリー化入門](http://qiita.com/KDKTN/items/6a27c0e8efa66b1f7799)は非常にわかりやすいので、一読をオススメいたします。

- [食べられないほうのカリー化入門](http://qiita.com/KDKTN/items/6a27c0e8efa66b1f7799)
- [カリー化談義](http://d.hatena.ne.jp/kazu-yamamoto/20110906/1315279311)
