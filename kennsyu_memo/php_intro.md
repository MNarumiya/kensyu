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


### defineで定数を定義する方法
```
bool define ( $定数名 , $値 [, bool $case_insensitive = false ] )
```

- defineは第一引数に定数名を指定し、第二引数に定数の値を指定
- PHP7から配列を指定することが可能
- defineで定義した定数は大文字小文字を明確に区別
  - 大文字小文字を区別しないようにするにはdefineの第三引数にtrueを指定する

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



#### クラス内定数
##### クラス内の定義方法
```
class クラス名{
  const 定数名 = 値;
}
```

#### 参考
- [【PHP入門】defineで定数を定義する方法](https://www.sejuku.net/blog/23526)
- [【PHP入門】constで定数を定義する方法](https://www.sejuku.net/blog/27679)
- [【初心者必見！】PHPのクラス(class)のさまざまな使い方を総まとめ！](https://www.sejuku.net/blog/25620)

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
