### forループを極める

#### for文を早くする

一般的なforループは以下のとおりです。

```js

for ( var i = 0; i < myarray.length; i++){
	console.log(i);
}

```

Nicholas Zakas著<a href="http://www.amazon.co.jp/gp/product/487311490X/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=487311490X&linkCode=as2&tag=kenjuw-22">『ハイパフォーマンスJavaScript』(O'Reilly)</a><img src="http://ir-jp.amazon-adsystem.com/e/ir?t=kenjuw-22&l=as2&o=9&a=487311490X" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
によると、配列や配列に似たオブジェクトに対してforループを処理する場合、以下のように配列の長さを一度変数にキャッシュしておく方法を紹介しています。これによって、毎回配列の長さを取得する必要がなくなります。

同著によれば、Safari 3で2倍、IE7で190倍速度が違うとのこと。個人的に開発してきた小規模のサービスでこの違いを大きく実感することはあまりありませんが、このようなTipsがあるのを知っておけば、大規模サービス開発の際になにかしら役に立つかもしれません。

```js

for ( var i = 0, len = myarray.length; i < len; i++){
	console.log(i);
}

```

また、ループ内でDOM要素を追加・削除するなど長さが変わってしまうような場合は
var文を外に出して宣言しておけば大丈夫です。

```js

function myFunc(){

	var i = 0,
		len,
		myarray = [];

	//...

	for ( i = 0, len = myarray.length; i < len; i++){
		// ...
	}

}


```

#### for文をもっと早くする

ここから先は少しテクニックに走りすぎている感もありますが、
コンペや、速度が命のネットワーキングあたりを書いているときなどでは
知っておくといいことがあるかもしれません。

以下のように、変数の数を減らし、カウントアップ方式からカウントダウン方式に
することによってより高速化を望めます。ただ、これはどの言語でも言えることですが、
速度を求めすぎたコードは可読性が下がりがちになるので、そのプロダクトに
求められるものを考えながらトレードオフで使い分ければいいかと思います。

```js

var i,
	myarray = [];

for ( i = myarray.length; i--;){
	//...
}

```