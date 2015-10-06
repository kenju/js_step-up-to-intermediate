### DOMで避けるべきこと

- DOMアクセスのループは避ける
	- 一度変数に格納してから
- 可能ならばセレクタAPIを使う
- DOMはid属性で活用
- DOM生成の回数はなるべく減らす

#### DOMアクセスのループは避ける

##### アンチパターン

```js

for (var i = 0; i < 1000; i += 1){
	$('#button01').innerHTML += i + ", ";
}

```

##### ベストプラクティス

```js

var i, content = "";
for ( i = 0; i < 1000; i += 1){
	content += 9 + ",";
}

// 最後にDOMにアクセス
$('#button01').innerHTML += content;

```

#### 可能ならばセレクタAPIを使う

ネイティブの`querySelector()`と`querySelectorAll()`は
自分でDOMメソッドを使って選択を実行するより早い。

```js

document.querySelector("ui .column");
document.querySelectorAll("#main .col-12");

```

#### DOMはid属性で指定

セレクタAPIを使わず自分で選択するときには
クラス名よりid名のほうがパフォーマンスが高い。

これについては簡単なので詳細省略。

#### DOM生成の回数はなるべく減らす

##### アンチパターン

```js

$('#button01').css(...);
$('#button01').animate(...);

```

このように同じ要素に対してDOM操作を繰り返す場合
一旦変数に格納して行う。

##### ベストプラクティス

```js

var btn = $('#button01');
btn.css(...);
btn.animate(...);

```