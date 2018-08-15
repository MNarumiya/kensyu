# PHPについて
## クラス
### クラス変数とインスタンス変数(::演算子について)
[侍エンジニアの記事](https://www.sejuku.net/blog/23753)から丸々コピペ

> 通常クラスの変数は外部から参照するためには、クラスのインスタンスを生成してクラスの関数や変数などにアクセスします。  
> しかし、前述したようにstaticを使用すれば、クラスのインスタンスを生成しないで直接クラスのメンバにアクセスすることができます。


> 言語によってはクラスメンバと言う場合もあります。その際、メソッドの場合はクラスメソッド、プロパティをクラス変数などと呼びます。
> その場合、逆に普通のメソッドやプロパティのことを、インスタンスメソッド、インスタンス変数などとも言います。  

#### 静的（static）メンバの使用方法
##### 静的（static）メンバの宣言方法
頭にstaticをつける(だけ)

```
クラス名{
  static $変数名 = 値;
  //処理
}
```
- staticにprivate, public, protectedアクセス権をつけられる

##### 静的（static）メンバのアクセス方法
- ::演算子でアクセス
- 静的なクラス変数は->演算子でもアクセスはできない

注意
- staticな変数を関数内で定義した場合、セットした値は処理が関数外に移っても保持

##### +Ultra
- staticな配列,関数も作成可能
- オーバライドができない(オーバーライドしようとしてもエラーは出ないが実行されてない)
- - 別の変数で初期化できない(これはconstも同様)(そのため式代入などはできない)

#### 動的メンバと静的メンバの関係性
- 静的なメソッドおよび静的なプロパティは、動的なメソッド・プロパティにアクセス可能.  
- 動的なメソッド・プロパティから静的メソッド、プロパティにアクセスは不可


#### 参考
- [【PHP入門】クラス変数・クラスメソッドを使用する方法(static)-侍エンジニア](https://www.sejuku.net/blog/23753)


### 定数について
- 定数は通常の変数と区別するために、基本的にアルファベットの大文字で記述するのが慣例(例:CONST_NAME)


### 定数の宣言方法
- defineで宣言
- constで宣言

> define関数とconst構文との違いは、define関数がプログラム内のどこからでも呼び出せるグローバルな定数を宣言できるのに対し、const構文は宣言したクラス内でしか使えないローカルな定数となっています。


### defineで定数を定義する方法
```
bool define ( $定数名 , $値 [, bool $case_insensitive = false ] )
```

- defineは第一引数に定数名を指定し、第二引数に定数の値を指定
- PHP7から配列を指定することが可能
- defineで定義した定数は大文字小文字を明確に区別
  - 大文字小文字を区別しないようにするにはdefineの第三引数にtrueを指定する
- クラス内での宣言は不可能

### constで定数を定義する方法
宣言方法:
```
class クラス名{

  const 定数名 = 値;

}
```

- [本質] **constはクラス内で定数を定義する** ...クラス外で定義するとエラー
- 変数の先頭に$の記号は付けない
- クラス内からconst定数を参照するときには「self::定数名」の形式を指定する
- 別の変数で初期化できない(これはstaticも同様)(そのため式代入などはできない)
- constとstaticは組み合わせて使えない



#### 他の定数を代入する
```
const 定数名1 = 値;
const 定数名2 = クラス名::定数名1;
```

#### +Ultra
- 配列定数を定義することも可能
- クラス外から定数を参照する ...方法は以下の3パターン
  - クラス名定数を::つなげて参照する方法
  - クラス名を定義した変数と定数を::で参照する方法
  - クラス変数に対して定数を::でつなげて参照する方法

  例
  ```
  <?php
  class ConstClass
  {
    //定数を定義する
    const FRUITS1 = 'apple';
    const FRUITS2 = 'orange';
    const FRUITS3 = 'melon';

  }

  echo ConstClass::FRUITS1;
  echo '<br>';

  $classname = 'ConstClass';
  echo $classname::FRUITS2;
  echo '<br>';

  $class = new ConstClass;
  echo $class::FRUITS3;

  ?>
  ```


#### defineとconstの違い
defineにしかできないことがある

- 動的な宣言
- 大文字小文字を区別しない宣言

#### 参考
- [【PHP入門】defineで定数を定義する方法](https://www.sejuku.net/blog/23526)
- [【PHP入門】constで定数を定義する方法](https://www.sejuku.net/blog/27679)
- [【初心者必見！】PHPのクラス(class)のさまざまな使い方を総まとめ！](https://www.sejuku.net/blog/25620)
- [【PHP入門講座】 変数と定数](https://qiita.com/mpyw/items/95e205056e25b7a59dfa)

### クラスのプロパティを取得する
クラスのデフォルトのプロパティのを取得するにはget_class_vars関数を使用.
これで取得できる値は,クラスのデフォルトの値のみで,コンストラクターでセットされた値には使えない.

使用方法:
```
array get_class_vars ( string $クラス名 )
```

使用例:
```
<?php

class sampleclass {

    var $item1;
    var $item2 = "ABCD";
    var $item3 = 100;
    public $item4 = 1000;
    private $item5 = 2000;

    // コンストラクタ
    function __construct() {
        //プロパティを設定する
        $this->item1 = "EFGH";
        $this->item2 = "300";
        return true;
    }
}

$sample_class = new sampleclass();

$class_array = get_class_vars(get_class($sample_class));

print_r($class_array);

?>
```

なお,この例の結果は
```
Array([item1] => , [item2] => ABCD, [item3] => 100, [item4] => 1000)
```

### クラスが定義済みか確認する(class_exists)
使用方法;
```
bool class_exists ( string $クラス名 [, bool $autoload = true ] )
```

引数:  
第一引数にはクラス名を指定します。クラス名は大文字と小文字を区別しません。  
第二引数にTRUEを指定すると、クラスのオートローディングを行います。
  - オートローダー...クラスのファイルを読み込んでなくとも自動でそのクラスがあるファイルを読み込む機能(PHP5以降から)
  - オートロードの例
    ```
    <?php
    spl_autoload_register(function ($class_name) {
        include $class_name . '.php';
    });

    $obj  = new MyClass1();
    $obj2 = new MyClass2();
    ?>
    ```

### 例外クラスについて
#### 参考
- [【PHP入門】Exceptionクラス徹底解説!例外をthrowしてtry~catchする](https://www.sejuku.net/blog/24907)


## JSONの取り扱い
json decodeは変数に対してのみ

### constでdefine
#### define
```
define('CLIENT_SECRET_JSON','{"web": {
  ...
  }
}');
define('CLIENT_SECRET_JSON_DECODES', json_decode(CLIENT_SECRET_JSON, true));
```

は可能



```
class ClientSecret{
  const _CLIENT_SECRET_JSON = '
		{
			"web": {
		   ...
		  }
		}';

  // 処理
}
```

```
public static function hogehoge(){
  return json_decode(self::_CLIENT_SECRET_JSON, true);
}
```

はいける

```
const _CLIENT_SECRET_JSON_DECODES = json_decode(CLIENT_SECRET_JSON, true);
```

はエラー

```
const _CLIENT_SECRET_JSON_DECODES = json_decode(self::_CLIENT_SECRET_JSON, true);
```

もエラー

```
public static function clientsecret_To_Json(){
  $CLIENT_SECRET_JSON_DECODE = json_decode(self::_CLIENT_SECRET_JSON, true);
  return json_decode(self::_CLIENT_SECRET_JSON, true);
  // return json_decode(CLIENT_SECRET_JSON, true);
}
```

はいける

## 三項演算子
- 三項演算子とは、1つの演算子処理で3つの式を使用するための比較処理
- 基本構文
  ```
  条件式 ? 真の式 : 偽の式
  ```
- 条件式の結果がTRUEの場合は真の式を返し、結果がFALSEの場合は偽の式を返す

###  用途
- 三項演算子の結果は変数に代入することが可能
  - 例:
    ```
    <?php
    $num1 = 100;
    $num2 = 200;

    $ans = $value1 < $value2 ? 'num1はnum2より大きい' : 'num1はnum2より小さい';

    echo $ans;
    ?>
    ```

- 三項演算子をメソッドの返り値としても利用可能
  - 例:
    ```
    <?php

    function foo($val){
      return  $value1 < $value2 ? 'num1はnum2より大きい' : 'num1はnum2より小さい';
    }

    $num1 = 100;
    $num2 = 200;

    //関数の呼出し
    $ans = foo($num1, $num2);
    echo $ans;
    ?>
    ```

### +Ultra:エルビス演算子とNull合体演算子
- PHP5.3から使えるようになったエルビス演算子とPHP7から使えるようになったNull合体演算子は、三項演算子を簡略化したもの

#### エルビス演算子
- 基本構文
  ```
  条件式(真の式) ?: 偽の式;
  ```
- 条件式がTUREと同等だった場合その値または「1」が返され、FALSEだった場合に偽の式が返される
- エルビス演算子は値がNULLの場合や、未定義(undefined)の時によく使われる

例:
```
<?php
$a = 0;
$num = $a ?: '0';

echo $num; // output: 0
?>
```

#### NULL合体演算子とは
- Null合体演算子は、条件式の結果がNULLかそうでないかを返す
- エルビス演算子の場合は変数宣言がされていないとエラーが表示されるが、Null合体演算子は、指定した変数が存在しない(変数宣言されていない)場合でも、エラーは発生せず値を返してくれる
- 基本構文:
  ```
  条件式または$変数 ?? 真の式;  // ?が2つ
  ```
  - 条件式または$変数の値がNULLのとき、真の式が返り、FALSEのときに条件式または$変数の値が返えされる

#### 三項演算子で検査関数(isset)を使用する
```
<?php
$value = NULL;

//isset関数で値を確認する
$ans = isset($value) ? '値が設定されています。' : '値が設定されていません。';

echo $ans;
?>
```

#### 雑談?
- 三項演算子よりif文での比較処理のほうが早い

#### 参考
- [【PHP入門】三項演算子とは？使いこなしてコードをスッキリする](https://www.sejuku.net/blog/23070)

## `htmlspecialchars`について
- HTMLエンティティ化する関数
- 書式:
  ```
  htmlspecialchars (エンティティ対象文字列, （フラグ, エンコード（これらは省略可））)
  ```


### HTMLエンティティ化とは?
以下の2つを比較

```
// HTMLエンティティ化した場合
echo htmlspecialchars('<a href="#">HTMLエンティティ化する。</a>');

// HTMLエンティティ化しない場合
echo '<a href="#">HTMLエンティティ化しない。</a>';
```

上記2つを比べると、以下のような違いがある
- HTMLエンティティ化した場合  
    htmlのリンクタグがそのまま文字列として表示され、クリックしても動作しない。
- HTMLエンティティ化しない場合  
    リンクのアンカーテキストだけが表示され、実際にクリックできる。

HTMLエンティティ化することで、上記サンプルのようにタグを無効化し、単なる文字列として出力してくれるので、悪意のあるスクリプトを仕込まれても大丈夫

### 参考
- [PHPのhtmlspecialcharsでのHTMLエンティティ化と、一文字に簡略化方法](https://www.flatflag.nir87.com/htmlspecialchars-555)

## PDOのprepareメソッドについて
PDOのprepareメソッド
- プリペアドステートメントとは、SQL文を最初に用意しておいて、その後はクエリ内のパラメータの値だけを変更してクエリを実行できる機能のこと
- この機能を利用することでクエリの解析やコンパイル等にかかる時間は最初の一回だけで良くなり、より高速に実行することが可能
- SQLインジェクション対策に必要なパラメータのエスケープ処理も自動で行ってくれるため、安全かつ効率の良い開発が可能

### プリペアドステートメントを利用してクエリを実行する流れ
1. PDOオブジェクトの作成
2. prepareメソッドでSQL文をセット
3. bindValue or bindParamでパラメータに値をセット
4. executeメソッドでクエリを実行
5. fetch or fetchAllメソッドで結果を配列で取得

### 例
```
//PDOオブジェクトの生成
$pdo = new PDO("mysql:dbname=test;host=localhost",USERNAME,PASSWORD);

//prepareメソッドでSQLをセット
$stmt = $pdo->prepare("select name from test where id = :id and num = :num");

//bindValueメソッドでパラメータをセット
bindValue("id",2);
bindValue("num",10);

//executeでクエリを実行
$stmt->execute();

//結果を表示
$result = $stmt->fetch();
echo "name = ".$result['name'];
```

bindValueを使わずにexecuteメソッドの引数に配列を使うことでセットする方法もある

```
//PDOオブジェクトの生成
	$pdo = new PDO("mysql:dbname=test;host=localhost",USERNAME,PASSWORD);

	//prepareメソッドでSQLをセット
	$stmt = $pdo->prepare("select name from test where id = :id and num = :num");

  //パラメータマークと値のペタの連想配列を作成
	$array = array("id"=>2,"num"=>10);

	//executeメソッドに配列を渡す。
	$stmt->execute($array);
	//結果を表示
	$result = $stmt->fetch();
	echo "name = ".$result['name'].PHP_EOL;
```

### 参考
- [PHPでPDOを使ってMySQLに接続、INSERT、UPDATE、DELETE、COUNT、SUM](https://qiita.com/tabo_purify/items/2575a58c54e43cd59630)
- [PDOでMySQLに接続してINSERTやUPDATEやSELECTやSUM](https://labo-iwasaki.com/code/pdo-index.html)

## ハッシュ化
何があっても`password_hash`でやるのが良さげ?

### 暗号化とハッシュの違い
- 暗号化は元の文字列に戻すことが可能で、そして元の文字列に戻すことが前提の情報に使用するもの
- ハッシュ化は一度変換したら元に戻すことのできない、一方通行の不可逆変換
  - 特定の入力値からは特定の出力値が出てくるので、その対応表さえ作ってしまえば10文字程度のパスワードであれば物理的に解読ができてしまう
  - そのようなクラックに対抗するためにSALTやStretchingといった手段が作り出されていて、それらを適切に実装するかぎりではパスワードはとても安全
  - password_hash関数は、同じパスワードを渡したとしても毎回異なる結果が出てくる
  - 実はpassword_hashはただのcryptのラッパーで、実際には関数内部でそのあたりの処理を行っているだけ
  - 認証は`password_verify`

### 参考
- [2018年のパスワードハッシュ](https://qiita.com/rana_kualu/items/3ef57485be1103362f56)

## ランダムな文字生成
### 暗号的に強い乱数のバイト文字列
```
string random_bytes ( int $length )
```

例
```
// ランダムな2バイトの文字列を返す。各バイトは0x00から0xffのどれか。
var_dump(bin2hex(random_bytes(2)));
```

- `string bin2hex ( string $str )` ...str を16進表現に変換したASCII文字列を返す


## 一意の文字列
`string uniqid ([ string $prefix = "" [, bool $more_entropy = false ]] )`

マイクロ秒単位の現在時刻にもとづいた、接頭辞つきの一意な ID を取(接頭辞を$pre
  で指定)  

### $prefixに乱数を指定する
```
<?php
	echo uniqid(random_int(0, 9999999999));
?>
```

 - `random_int( 最小値, 最大値)` は必ず最大値最小値を指定

### $prefixにランダムなバイト文字列を指定する
```
<?php
	echo uniqid(bin2hex(random_bytes(8)));
?>
```

### 参考
- [PHPでユニークなIDを生成する：uniqid()](https://kakakakakku.hatenablog.com/entry/20081016/1224154493s)


## header関数
- header関数は、HTTPヘッダー情報を送信するときに使用
  - HTTPヘッダーとは、HTTPによる「リクエスト（要求）→レスポンス」の流れで、どのような情報をリクエストして、どのようなコンテンツを受け取るかを定義するためのもの
- 以下のように定義
  ```
  header ( $ヘッダ文字列 [, bool $replace = true [, int $http_response_code ]] )
  ```

### やれること
- ファイルをダウンロードする
- 指定したページにリダイレクトする
- JSONを出力する

### 参考
- [【PHP入門】header関数の使い方まとめ](https://www.sejuku.net/blog/28054)
