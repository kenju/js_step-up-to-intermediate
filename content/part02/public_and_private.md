### パブリック/プライベートを意識する

まず、次のコードを見てください。
`User()`関数は、`user`オブジェクトをプライベートメンバに
持っており、容易にIDやemailなどが変更されないようにしたい。

また、プライベートメンバにアクセスできる`getUser()`メソッドを準備しました。

（注）これはアンチパターンです。真似しないでください。

```js

function User(){
  // プライベートメンバ
  var userInfo = {
    userId: "user01",
    userEmail: "user01@gmail.com",
    userName: "David Brown"
  };

  // パブリック関数
  this.getUserInfo = function(){
    return userInfo;
  };
}

```

この設計には問題が有ります。
以下の呼び出し方を見てみてください。

```js

var user = new User(),
    userInfo = user.getUserInfo();

userInfo.email = "evil@gmail.com";
userInfo.nickname = "Hello, world!";

console.dir(user.getUserInfo());

```

このようにすることによって、本来はプライベートメソッドである
userInfoの中身が書き換えられてしまいました。

このような振る舞いを避けるために、プライベートにしておきたいオブジェクトや配列に対する参照を渡さないようにするほかありません。

たとえばこの場合、userIdとuserNameだけを渡すメソッドを作成したりすれば良いでしょう。

```js

function User(){
  // プライベートメンバ
  var userInfo = {
    userId: "user01",
    userEmail: "user01@gmail.com",
    userName: "David Brown"
  };

  // パブリック関数
  // * すべての秘匿情報を返さない（必要な物だけ返す）
  this.getUserId = function(){
    return userInfo["userId"];
  }
}

```