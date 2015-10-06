### 高速化・最適化のためのTips

#### Stylesheet > JavaScriptの順で読み込む

- scriptタグを読み込んでいる間ページはブロックされてしまう
- 下部で読み込むことによって、ページ内要素がレンダリングされてからscriptが読み込まれるので、ユーザは真っ白ページで数秒待たされるということがない
- 特に画面描画に関係ないJSは</body>の直前に

#### CSSやJavaScriptは外部ファイル or CDNで

- 通常HTMLは毎回ダウンロードされるが、JavaScriptなどはファイルはキャッシュされるため
- CDNが使えるのならCDNを利用する
	- ミスが許されないプロジェクトの場合、CDNが読み込めなかった場合の処理も行う

#### 参考リンク
- [JavaScript・jQueryの改修・高速化のためのメモ](http://qiita.com/redamoon/items/1c38ee93834588e3c4ff)