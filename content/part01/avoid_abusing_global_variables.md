### グローバル変数を乱用しない

JavaScriptでは「グローバル汚染」という言葉を耳にするほど、グローバル変数による名前空間の汚染が問題となることがあります。グローバル変数やスコープをそもそもわかっていない場合、もしくは言語的に正しい文法はかけているが、グローバル変数を多用することによるデメリットを理解していない場合があります。

#### グローバル汚染の悪影響

グローバル空間を特に理由のない変数定義で汚染してしまうと、

- サードパーティプラグインを読み込んだ時の変数の衝突
- チームメンバーが書いたコードとの名前衝突
- 昔書いた自分のコードで使った変数との衝突

が起こってしまうことがあります。したがって、以下の方法を使ってグローバル変数を使わないための工夫をする必要があります。

#### 対策：varを忘れない

JavaScriptでは、varを使わずに定義した変数はグローバル変数として走査されます。

```js

function speakOut(){
  // global variable
  global = "Hello from global";

  // local variable
  var local = "Hello from local";

  console.log(global);
  console.log(local);
}

speakOut();

console.log(global);
console.log(local); // -> これだけエラーをはく


```

#### 対策：名前空間を利用する

```js

// object for name space
var myApp = {};

myApp.name = "My First JavaScript App";

```

#### 対策：クロージャを利用する

```js

(function(){
  maybe_global = "Hello from global?"; // varを忘れるとグローバル
  var local = "Hello from local";

})();

console.log(maybe_global);
console.log(local);

```

#### ベストプラクティス：単独varパターン

以下のように、必ず関数の先頭でvar文を書くことによって、誤ってローカル変数をvarを付け忘れて宣言してしまうことを避ける事ができます。

- 習慣付けによってローカル変数のvarつけ忘れを防ぎやすい
- ローカル変数や関数自体の全体的な可読性があがる
- コード量が少なくて済む

```js

function login(){
	var userId,
		userPasswd,
		date,
		sessionId,
		errorMessage = [],
		status = {};

	...

}

```