### 即時関数

即時関数は、使い方と理解は簡単です。「関数を作成した直後に実行される関数」です。以上。

。。。

ですが問題は、その「使いどころ」です。即時関数自体は知っていても、**「いつ使うのか」、「なんのために使うのか」** を知らなければその真価を発揮させることはできません。

即時関数は、 **「一回だけしか必要ない処理」** をするために使われます。具体的には
- ページが読み込まれたときの初期化処理
- オブジェクトの生成
- DOM要素へのイベントハンドラの設定

などです。これらの処理は一回しか実行されないので、再利用可能な名前付き関数を作る理由は全くありません。むしろそうしてしまうと、グローバルスコープを汚染してしまいます。そう、即時関数とは、 **「一回だけ必要な処理を、グローバルスコープを汚染せずに実行する方法」** なのです。

具体的には、ブックマークレットなどでよく使われます。仕事を効率化させるような便利なブックマークレットが多いので、以下のリンクを参考にしてください。

- [100+ Useful Bookmarklets For Better Productivity | Ultimate List](http://www.hongkiat.com/blog/100-useful-bookmarklets-for-better-productivity-ultimate-list/)
- [Top 10 Useful Bookmarklets](http://lifehacker.com/395697/top-10-useful-bookmarklets)


以下が代表的な即時関数です。

```js

(function() {
  //...
}());

```

ちなみに、即時関数は以下の書き方もできますが、JSLintでは上記のやり方が推奨されています。

```js

(function(){
  //...
})();

```

#### 基本的な使い方

##### 即時関数に引数を渡す

以下のように即時関数に引数を渡すことができます。

```js

(function(firstname, lastname){
  console.log("Hello, " + firstname + lastname + "!");
}("David", "Brown"));

```

『JavaScriptパターン』の引用ですが、一般には、即時関数の引数にはグローバルオブジェクトを渡します。なぜならば、そうるすことによって`window`を使わずに関数の内部からアクセスできるからとのこと。

```js

(function (global) {

  // globalを介してグローバルオブジェクトにアクセス

}(this));

```


#### 参考リンク
- [即時関数のメリットと主な用途](http://analogic.jp/immediate-function/)
- [JavaScriptで即時関数を使う理由](http://qiita.com/katsukii/items/cfe9fd968ba0db603b1e)
