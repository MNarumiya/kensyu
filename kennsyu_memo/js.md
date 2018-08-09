# JavaScriptについて
## 関数の書き方
```
function hogehoge() {
  //処理
}
```

- 呼び出しは`hogehoge();`

### 無名関数
関数も1つのデータ型なので,変数に代入できる

```
var fugafuga = function() {
  //処理
};
```

- セミコロンをつけるのを忘れずに
- 呼び出しは`fugafuga();`でできる

### 即時関数
宣言してすぐ呼び出しする際は,即時関数として使える

```
(function hogehoge() {
  //処理
})();
```

- もちろん以下のようにして引数も取れる
  ```
  (function hogehoge(name) {
    //処理
  })("Tom");
  ```

- もちろん無名関数にもできる
- 無名関数は変数を安全に使うことにも使える  
  例えば以下のようにすれば,xがローカル変数として使えるので,他のアクセスを防げる.
  ```
  (function() {
    var x =10;
  })();
  ```

### jQuery
始まりの書き方の基本は`$(function(){`から始まる.  
これは,「ページが読み込まれたら,ここを読み込んでね」という元々の長い記法の省略記法

- セレクタ :処理対象となるDOM要素を指定する記法
- メソッド : 処理
  - メソッドを繋げて書くことができる...メソッドチェーン

例:
```
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
<script type="text/javascript">
	$("p").css('color,'red);
</script>
```

pがセレクタ,.css()がメソッド

### 動的に割り当てられた要素に対してイベントを割り当てる
onメソッドを用いる
