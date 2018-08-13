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

### foreachについて
- 配列データに特化した繰り返し処理を簡単に実行できるメソッド
  - for文はループの終了条件やカウンターなどの設定が必要なので少し面倒(foreachはいらない)
  - コールバック関数も使えるので複雑な条件分岐処理もスマートに実装できる
- forEachは「for文」と違い、繰り返し処理を中断する「break / continue」が使えない
  - 使うとerrorになる
  - 使いたいときはfor文を使う
- 任意のオブジェクトをコールバック関数のthisとして指定することが可能！
- 類似メソッドとして「$.each」「map」がある

- 書式
  ```
  var array = [配列データ];
  array.forEach( コールバック関数 )
  ```
-  例
  ```
  var items = [ 'バナナ', 'リンゴ', 'メロン', 'ブドウ' ];

  items.forEach(function( value ) {
    console.log( value );
  });
  ```

#### オブジェクトの配列の中身を取得
例  
```
var lists = [
    { name: 'バナナ', price: '100' },
    { name: 'リンゴ', price: '200' },
    { name: 'メロン', price: '400' },
    { name: 'ブドウ', price: '250' },
];

// オブジェクトの配列の中身を取得
lists.forEach( function( value ) {
    console.log( value.name );
});
```
#### 「forEach文」のコールバック関数について
##### 基本的なコールバック関数の使い方
```
array.forEach( function( value, index, array ) {
  // 繰り返し処理を書く
});
```

- コールバック関数は「引数」として3つの値を受け取ることが可能
- valueは配列データの値, indexは配列のインデックス番号, arrayは現在処理している配列
- 例: 配列の「元データ」をすべて2倍にして上書きするプログラム
  ```
  var lists = [ 1, 2, 3, 4, 5, 6, 7, 8 ];

  lists.forEach( function( value, index, array ) {
    array[ index ] = value * 2;   
  });
  ```

##### 「第2引数」にオブジェクトを設定する
```
array.forEach( コールバック関数, オブジェクト )
```

- 任意のオブジェクトをコールバック関数内の「this」として設定することができる

- 例:
  ```
  var priceLists = {
      'バナナ': 100,
      'リンゴ': 200,
      'メロン': 400,
      'ブドウ': 250
  }

  var lists = [ 'バナナ', 'ブドウ' ];

  lists.forEach( function( value ) {

      // この「this」は、「priceLists」を参照する
       console.log( value + 'の価格は' + this[value] + '円です' );

  }, priceLists ) // ここが肝
  ```

##### 「forEach文」と「$.each」の違い
jQueryにも似たメソッドが存在する...eachメソッド

違いは以下のよう
- コールバック関数の引数の構成が異なる
  - jQueryのeachメソッドは以下の通りコールバック関数(function)の第一引数は「index」、第二引数は「value」と指定する位置が逆
  - 例:
    ```
    $.each(array,function(index,value){
      // 繰り返し処理
    });)
    ```

- 繰り返し処理から抜ける方法が異なる
  - jQueryのeachメソッドではループ処理（繰り返し処理）を途中で止めるためにreturn falseを記述することで実現可能
  - JavaScriptのforEach文は処理を途中で止めることができない
  - JavaScriptにて配列の要素の繰り返し処理を途中で止めるためにはforEachメソッドの代わりにsomeメソッドを利用する
  - someメソッドは配列の要素を順に処理する中で、コールバック関数内で、return true を返すことで処理を途中で止めることが可能
    - 参照: [【JavaScript入門】配列の要素を検索する4つの方法！](https://www.sejuku.net/blog/22228#some)

#### 「forEach」と「map」の違い
- どちらも配列データに対して繰り返し処理ができる
- 記述方法もほとんど同じ
- 決定的に違うのは「返り値」があるかどうか(mapは返り値がある)

- 例: 2倍にした値を返り値として新しい配列「newArray」に格納
  - forEachでnewArrayに格納
    ```
    var array = [1,2,3,4,5];
    var newArray = [];

    array.forEach(function( value ) {
        var result = value * 2;
        newArray.push( result );
    })
    ```

  - mapでnewArrayに格納
    ```
    var array = [1,2,3,4,5];

    var newArray = array.map(function( value ) {
        return value * 2;
    })
    ```

#### 参考
- [【JavaScript入門】「forEach文」の使い方を基本から応用まで徹底解説！](https://www.sejuku.net/blog/20257)

### `documnet.cookie`について(cookieについて)
#### 「cookie」とは？
- 一般的にWebサイトに訪問してきたユーザーの情報を保持するために使われる機能
- 「cookie」がどのような情報を保持しているかは、各種ブラウザのデベロッパーツールから確認できる
  - Google Chromeの場合,`https://...`とURLの書いてある左横の「保護された通信」とか書いてあるところをクリックすれば,見られる
- 普通は大文字っぽい

#### cookieの使い方
##### cookieにデータを書き込み
- 一般的な書き込みの方法としては、【 cookie名 = 値 】
- 例: nameとしうcookie名に太郎を入れる
  ```
  document.cookie = 'name=太郎';
  ```
- 注意:cookieへセットする値に「セミコロン」や「カンマ」もしくは「空白」を含めてはいけない
  - ゆえに,「encodeURIComponent()メソッド」を使用して値をエンコードしてからセットすると安全
    - 例:
    ```
    document.cookie = 'cookie_name=' + encodeURIComponent('outlet shoes');
    ```

##### cookieにデータの取得
- 「cookie」に格納されているデータは単純な文字列（String型）
- 文字列を分割できる「split()」メソッドを利用
  - 例:
    ```
    //データを1つずつに分ける
    var r = document.cookie.split(';');

    // 各cookieに対して,cookie名と値に分けて(content[0]がcookie name),cookieの値の方を表示
    // rの1要素が引数valueに入れられてる(jsのforeachの使い方っぽい)
    r.forEach(function(value) {
        var content = value.split('=');
        console.log( content[1] );
    })
    ```

- 特定のcookieの値を読み取る
  ```
  function getCookie(key) {
	var cookieName = encodeURIComponent(key) + "=",
            cookieStart = document.cookie.indexOf(cookieName),
            cookieValue = null,
            cookieEnd;

				// 特定のキーの値を読み取る
        if (cookieStart > -1){
            cookieEnd = document.cookie.indexOf(";", cookieStart);
						// ;が見つからなかったら最後の値なのでEndは最後
            if (cookieEnd == -1){
                cookieEnd = document.cookie.length;
            }
            cookieValue = decodeURIComponent(document.cookie.substring(cookieStart + cookieName.length, cookieEnd));
        }

        return cookieValue;
  }
  ```

- `document.cookie.substring`について
  - substringメソッドの利用...Stringオブジェクトの組み込みメソッド
  - 書式:
    ```
    var str = '文字列';
    str.substring(開始位置, 終了位置);
    ```
  - インデックスは0から
  - 終了位置にある文字は含まれない
  - 開始位置のみを指定した場合は,開始一から最後まで
  - 開始位置, 終了位置は負の値は取らない(負の値を入れたら全て0に変換される)
  - 末尾から取得したい場合はlenghプロパティを用いる
    ```
    var number = '047-123-5678';
    var result = number.substring(number.length - 4);
    ```
  - 「slice」メソッドでも同じことができる(Stringも配列として扱えるから

  - **indexOfメソッドで文字列を検索して抽出する**
    - 例:
      ```
      var str = 'イチゴバナナメロン';

      //「バナナ」の文字位置を取得する
      var target = str.indexOf('バナナ');

      //「バナナ」移行の文字を取得する
      var result = str.substring(target);

      console.log('バナナの位置：' + target);  // output: バナナの位置：3
      console.log(result); // output:
      ```

  - **indexOfメソッドで文字列を検索**
    - 例:
      ```
      var str = 'user-1, user-2, user-3, user-4, user-5';
      var target = str.indexOf( 'user-3' );

      if(target !== -1) {
        console.log(str.substring(target));
      }
      else {
        console.log('存在しません');
      }

      // output: user-3, user-4, user-5
      ```

##### cookieのデータを削除する
- 「cookie」には有効期限を設定することができる
  - 期限が過ぎると「cookie」のデータが自動的に削除される
  - JavaScriptには「cookie」を削除するメソッドが無いので、実際にはこの「有効期限」を活用することで意図的に削除することが可能
- 有効期限は【max-age=秒数】のように、秒数を指定することで設定が可能
  - 例:
    ```
    // 即座にcookieを消す
    document.cookie = 'name="太郎"; max-age=0';
    ```

##### cookieの設定
- 大きく分けると、「有効期限」「有効範囲」「セキュリティ」の3つのオプションを設定することが可能
- 有効期限について
  - `max-age` ...指定した秒数だけ有効期限になる
  - `expires` ...指定した日付までが有効期限になる.
    - セキュリティの観点からも、半永久的にユーザー情報を保持しているのは良くないため期限を設定するのが慣例
    - 「GMT（グリニッジ標準時）」形式で指定する必要がある.
    - GMT形式に変換するために「toUTCString()」メソッドを利用すればOK
- 有効範囲について
  - `path` ...指定したパスがcookieの有効範囲になる
  - `domain` ...指定しドメインがたcookieの有効範囲になる
- セキュリティについて
  - `secure` ...https通信の時だけcookieが有効になる


例: 期限をGMTで指定
```
//有効期限の日時を指定する
var count = new Date('2018/3/10 18:00');

//GMTで期限を設定する
document.cookie = 'name=太郎; expires=' + count.toUTCString();
```

##### cookieの判定処理
- 任意のデータがすでに格納されているかどうかを判定したい時に用いる
- 「indexOf()」メソッドを用いる
- 書式:
  ```
  document.cookie.indexOf(調べたいcookie name);
  ```
- 返り値: setされてなければ-1, 見つかった場合は何文字目に存在しているかを数値で帰る

例: numberという名前のcookieにアクセス
```
document.cookie = 'number=123';

// 「indexOf()」を使ってcookie内に「number」という文字列が含まれているかどうかをチェック
var r = document.cookie.indexOf('number');
console.log(r);
```

#### 参考
- [【JavaScript入門】cookieの設定・取得・削除まとめ！](https://www.sejuku.net/blog/28696)
- [Cookieに書きこまれたデータを読み込む (JavaScript プログラミング)](https://www.ipentec.com/document/javascript-read-cookie)
- [【JavaScript入門】substringで文字列の切り出しを行う方法まとめ！](https://www.sejuku.net/blog/25730)

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
  $(function() {
      $("p").css('color,'red); // 処理
    });
</script>
```

pがセレクタ,.css()がメソッド

### 動的に割り当てられた要素に対してイベントを割り当てる(onメソッドについて)
onメソッドを用いる  
- 書式
  ```
  対象セレクタ-.on( イベント名, セレクタ, 関数 )
  ```

- 主なイベント名
  - change ...フォーム部品の状態に何らかの変化があった時に発動
  - click ...要素がクリックされた時に発動
  - blur / focus ...要素にフォーカスが当たったとき(focus)、外れたとき(blur)に発動
  - load ...ドキュメントが読み込まれたあとに発動
  - resize ...画面がスクロールした時に発動
  - keyup / keypress	...キーボードのキーが押された時(keypress)、離された時(keyup)に発動
  - mouseup / mousedown ...マウスのボタンが押された時(mousedown)、離された時(mouseup)に発動
  - submit ...フォームが送信された時に発動
  - error ...何らかのJavaScriptエラーが発生した時に発動

- 「on()」は同じ要素に複数のイベントを追加していくことができる

#### 「on()」の活用
##### 基本的なclickイベント処理の方法
```
<body>
<button>ボタン</button>

<script>
    $('button').on('click', function() {

        console.log('クリックされました！');

    })
</script>
</body>
```

#### 複数イベントの扱い方
```
<body>
    <button>ボタン</button>

    <script>
        $('button').on('click mouseenter', function() {
            console.log('クリックされました！');
        })
    </script>
</body>
```

#### イベントにオブジェクトデータを渡す方法
- 「on()」メソッドの引数にはオブジェクト形式のデータを指定することも可能
- このデータは、関数に渡すことが可能なので任意のデータを活用した処理ができるようになるので便利
- データにアクセスするには、「e.data.プロパティ名」のように記述すること実現
  - e以外だとどうなる?

```
<body>
    <button id="one">ボタン１</button>
    <button id="two">ボタン２</button>

    <script>
        $('#one').on('click', {name: '太郎'}, showText);
        $('#two').on('click', {name: '花子'}, showText);

        function showText( e ) {
            console.log( e.data.name + 'さん、こんにちは！' );
        }
    </script>
</body>
```

#### デリゲートを使ったイベント登録
- 「デリゲート」は、簡単に言うとイベントが発生した要素が別の要素にイベント処理を任せてしまうことができる機能
- デリゲートを使うとイベント処理のリソースを効率化できる
- 要は親要素にセレクタを任せてしまう機能?

例:
```
<body>
<ul>
    <li>リスト１</li>
    <li>リスト２</li>
    <li>リスト３</li>
</ul>

<script>
    $('li').on('click', function() {
        console.log('クリック');
    })
</script>
</body>
```

ではなくて

```
<body>
<ul>
    <li>リスト１</li>
    <li>リスト２</li>
    <li>リスト３</li>
</ul>

<script>
    $('ul').on('click', 'li', function() {
        console.log('クリック');
    })
</script>
</body>s
```

#### 「on()」と「click()」の相違点
類似点
- 「on() / click()」のイベント処理は、最初から存在しているHTML要素と紐づく特性がある

違う点
- 「click()」はクリックイベントしか検出できないのに対して、「on()」はあらゆるイベント処理を記述が可能
- 「click()」はjQueryで途中から追加されたHTML要素を検出できる(しかし,工夫が必要)

#### 参考
- [【jQuery入門】on()によるイベント処理の使い方まとめ！ | 侍エンジニア塾 ](https://www.sejuku.net/blog/38774)

### `$.post()`について
- 「post()」は、ブラウザ側から任意のデータをサーバへ送信してその結果を取得することができるメソッド
- 例えば、サーバー側で受け取ったデータを処理してJSONに変換してまたブラウザ側で受け取るようなことも簡単に実現可能

#### 書式
```
$.post( サーバーへのパス, 任意のデータ );
```

サーバーからの返信を受け取る
```
$.post( サーバーへのパス, 任意のデータ )

//サーバーからの返信を受け取る
.done( function(data) {  } )

//通信エラーの場合
.fail( function() {  } )

//通信が終了した場合.通信異常に関係なく必ず実行される処理
always ( function() {  } )
```

もっと細かくいうと(っていうかこのタイプは古いらしい)
```
$.post( url [, data] [, success] [, dataType] )
```

- url ...リクエストの送信先URLを指定
- data ...サーバに送信する値をマップ値で指定.サーバーに送信される前にURLエンコードが施されたURLクエリーの文字列に変換.
- success ...リクエスト成功時の処理を関数として指定.関数の引数としてdata, textStatus, jqXHRnの3つの値を受け取る事が出来る.
- dataType ...サーバから返されるデータ方式(xml, json, script, html)を指定

#### Formによる送信方法
結果的なサンプルコード
```
<form>
    <input type="text" name="sample">
    <input type="text" name="sample">
    <input type="text" name="sample">

    <input type="submit" value="送信">
</form>

<script type="text/javascript">
$('form').submit(function( event ) {
    event.preventDefault(); // 送信ボタンをクリックしてからpost()メソッドを実行させるようにsubmitが実行されると、画面が必ず更新されるというブラウザの仕様をキャンセル
    //post()の処理
    $.post( 'https://httpbin.org/post', $('form').serialize() )

    //サーバーからの返信を受け取る(JSONオブジェクトで取得)
    .done(function( data ) {
        console.log( data.form );
  })
})
</script>
```

##### サーバーへ送信するためにフォームの「データ形式」
```
パラメータ名=文字列データ&パラメータ名=文字列データ・・・
```

- 「パラメータ名」とはinputタグのname属性値
- 「文字列データ」とはユーザーが入力した文字列
- 複数のinputタグがある場合は「&」で連結

##### サーバーに送信する形にする
- jQueryの標準で提供されている「serialize()」メソッドを使うと送信用のデータ形式に変換

```
<form>
    <input type="text" name="sample1" value="hello">
    <input type="text" name="sample2" value="world">
</form>
<script type="text/javascript">
$('form').serialize();
</script>
```

これで,

```
sample1=hello&sample2=world
```
の形になる

##### Formのデータを利用したpost()の書き方
```
<form>
    <input type="text" name="sample">
    <input type="text" name="sample">
    <input type="text" name="sample">

    <input type="submit" value="送信">
</form>

<script type="text/javascript">
$.post( 'https://httpbin.org/post', $('form').serialize() )

.done(function( data ) {
    console.log( data.form );
})
</script>
```

しかし,このままだとHTMLが読み込まれた瞬間にpost()が実行されてしまう

##### 送信ボタンをクリックしてからpost()メソッドを実行させるようにする
```
$('form').submit(function( event ) {
    event.preventDefault();

    //post()の処理をここに記述する

})
```

#### 参考
- [【jQuery入門】post()でデータを送信・取得する方法！](https://www.sejuku.net/blog/42985)
- [$.post() | jQuery 1.9 日本語リファレンス | js STUDIO](http://js.studio-kingdom.com/jquery/ajax/post)
