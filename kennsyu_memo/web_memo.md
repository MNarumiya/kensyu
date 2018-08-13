## Google ログイン

-   Google OAuthを用いる

### 参考記事

-   [Google OAuth 2.0 認証を使ったログインの実装](https://qiita.com/kite_999/items/bddd62c395f260e745bc)


途中から他のやつをつかった.  
jsonにまとめてある.(多分,ダウンロードしたやつに入ってる)

### googleログインメモ

-   Googleプラットフォームライブラリ
    `<script src="https://apis.google.com/js/platform.js" async defer></script>`を実装?

-   google-signin-client_idメタ要素を使用して、Google Developers Consoleでアプリ用に作成したクライアントIDを指定
    `<meta name="google-signin-client_id" content="YOUR_CLIENT_ID.apps.googleusercontent.com">`

-   googleログインボタンを設置
    `<div class="g-signin2" data-onsuccess="onSignIn"></div>`

-   サイン後,ユーザー情報取得

        function onSignIn(googleUser) {
        var profile = googleUser.getBasicProfile();
        console.log('ID: ' + profile.getId()); // Do not send to your backend! Use an ID token instead.
        console.log('Name: ' + profile.getName());
        console.log('Image URL: ' + profile.getImageUrl());
        console.log('Email: ' + profile.getEmail()); // This is null if the 'email' scope is not present.
        }

#### error

##### 1 `Error: redirect_uri_mismatch`

詳細は以下.

    The JavaScript origin in the request, http://localhost, does not match the ones authorized for the OAuth client. Visit https://console.developers.google.com/apis/credentials/oauthclient/189770898262-2b9e9jjpr991qub198gds5ovs5tivcse.apps.googleusercontent.com?project=189770898262 to update the authorized JavaScript origins.

そもそも,redirect_uriはどこでで指定されてるんだよ...  

試して見たこと  

-   `https://console.developers.google.com/apis/credentials/oauthclient/189770898262-2b9e9jjpr991qub198gds5ovs5tivcse.apps.googleusercontent.com?project=189770898262`に行って「承認済みの JavaScript 生成元」に<http://localhostを登録しても何も変わらず>.
    ...あ,きたw  
    redirect_uriから<http://localhostを消したらできたww>  

###### 2.JSのConsoleエラー

`Uncaught {error: "idpiframe_initialization_failed", details: "Not a valid origin for the client: http://localhos…it`

エラーが変わった

    Uncaught
    Object
    details:"Not a valid origin for the client: http://localhost has not been whitelisted for client ID 189770898262-2b9e9jjpr991qub198gds5ovs5tivcse.apps.googleusercontent.com. Please go to https://console.developers.google.com/ and whitelist this origin for your project's client ID."
    error:"idpiframe_initialization_failed"

いや同じか.

## 次回方針

-   facebook連携もしてみる
-   [ここ](http://rmdi.hatenablog.com/entry/2015/12/22/142059)をみて,最後のサーバー側に送るやつを見てみる

雑メモ

-   今回ひかかってるところはJSの問題では?
-   というわけでPHP主体に戻すのはGoogleさんのHPよりも,調べて出てきたやつをやってそのエラーを直す方がPHPとしては筋が良い???

* * *

## 問題を切り分けて突き止めていく

## APIについて

### 定義

> 「API」とは、アプリケーション・プログラミング・インタフェース（Application Programming Interface）の略です。狭義にはアプリケーション作成に利用するもので、ある外部アプリの機能を共有するシステムのことです。

### 「インターフェース」の定義

> ソフトウェアでは、プログラム間でデータをやり取りするために定められた仕様やAPIなどがある。（コトバンクより引用）

つまり,利用者とプログラムの間にあるものが「インターフェース」

API = 機能 + 説明書

### slack api

#### 種類

-   レガシ-...機能制限なし.これ古いらしい
-   レガシーじゃない...機能ごとに一つ一つレガシーを発行

### composerコマンド

composerとはPHPのパッケージ管理システムのこと  
composerを使うとコマンド一発で依存ファイル（Bライブラaリ、Cライブラリ）も含めダウンロードしてくれる

#### composerのインストール方法

環境

-   macOS High Sierra
-   PHP 7.0.28

#### 出会ったエラー

    $ brew install homebrew/php/composer

とすると

    Error: homebrew/php was deprecated. This tap is now empty as all its formulae were migrated.

となった

    php composer-setup.php --install-dir=bin --filename=composer

も,

    PHP Warning:  PHP Startup: Unable to load dynamic library '/opt/local/lib/php70/extensions/no-debug-non-zts-20151012/php_openssl.dll' - dlopen(/opt/local/lib/php70/extensions/no-debug-non-zts-20151012/php_openssl.dll, 9): image not found in Unknown on line 0
    Could not open input file: composer-setup.php

#### できた方法

    $ brew install composer

参考文献

-   [Homebrew で Apache, PHP, MySQL, Composer をインストールして Yii Framework を動かすところまで
    ](https://qiita.com/livejam_db/items/b70caccdeece036a3797)

### 参考

-   [APIとは | 連携の具体例・メリット・実装手順を紹介](https://boxil.jp/mag/a2822/)

## facebookログイン

> Facebookログインを実装するには以下の手順に従います。
>
> 1.  リダイレクトURLを入力し、利用者をsuccessful-loginページに誘導します。
> 2.  ログインステータスを確認します。利用者がアプリにログイン済みかどうかを確認します。 このステップでは、現在はログインしていないが以前にアプリにログインしたことがあるかどうかも確認する必要があります。
> 3.  ログインボタンかログインダイアログで利用者にログインを促し、一連のデータへのアクセス許可をリクエストします。
> 4.  利用者にログアウトを促し、アプリを終了します。

### まず前提

> facebookログインとか実装するとき， ドメインを指定してトークン(?)を取得します．
> そのため開発環境へのアクセスをlocalhostとか192.168.33.10とかにしてるとエラーでます．
> そこで開発環境にドメイン名でアクセスする必要があります．

### 各種キー?

アプリID : 1949929875059913  
app secret: 64b7190982941b99761d9c4eec8a826d

### 公式リファレンスでハマった

[公式リファレンス](https://developers.facebook.com/quickstarts/1949929875059913/?platform=web)にしたがって,

    <?php ?>

    <html>
    <head>
    </head>

    <body>
      <h1>Facabookでログイン Test</h1>
      <script>
      (function(d, s, id){
         var js, fjs = d.getElementsByTagName(s)[0];
         if (d.getElementById(id)) {return;}
         js = d.createElement(s); js.id = id;
         js.src = "https://connect.facebook.net/en_US/sdk.js";
         fjs.parentNode.insertBefore(js, fjs);
       }(document, 'script', 'facebook-jssdk'));

        window.fbAsyncInit = function() {
          FB.init({
            appId      : '{1949929875059913}',
            cookie     : true,
            xfbml      : true,
            version    : '{v3.1}'
          });

          FB.AppEvents.logPageView();
        };
    </script>

      <div
      class="fb-like"
      data-share="true"
      data-width="450"
      data-show-faces="true">
    </div>
    </body>

    </html>

とすると「いいね」がでるはずなのに出ない.  
調べても分からない.そもそもJSについて知らないので,そこでエラーが出ても分からない.「一度JSするかあ...」とJSを学ぼうとしていたところ,社員さんが助けてくれた.  


### デバック方法

Chromeのディベロッパーツールを使う.  
正確には,JSのデバッカーを用いる.  

### 結論

appIDの{}や,versionの{}がいらなかっただけだった

### 次はログインボタンとログアウトボタンの設置を目指す

ログアウトはアプリからのログアウトと考える.  
fbで登録とログインを分けて,登録の際はDBに新しく作る.  
この際JSからPHPに変数の値を渡す.

#### JSからPHPに変数を渡す

PHPの公式リファレンスには以下のようにある.

> Javascript は（普通は）クライアントサイド技術であり、一方 PHP は（普通は） サーバーサイド技術です。また HTTP は"ステートレスな"プロトコルです。 そのため、この二つの言語はダイレクトに変数を共有することができません。
>
> しかしながら、この二つの言語の間で変数を渡すことは可能です。 一つの方法は PHP と一緒に Javascript のコードを生成し、 ブラウザに自動的にリフレッシュ（再ロード）させることです。

とPHPからjavascriptを制御する方法は書かれてるが,今知りたいのは逆のこと.

### jQueryとは

> jQueryをざっくりと説明すると「JavaScript」と呼ばれるプログラムをより扱いやすくしたファイルのことです。（通称：ライブラリ）
> ([jQueryってなに？超初心者向け入門講座](https://webkikaku.co.jp/blog/webdesign/jquery_start/)より抜粋)

### jsについて

dotinstall

#### DOMについて

`console.dir(オブジェクト名)`でオブジェクトのプロパティを見られる  
`window.doument`...今開いてるページのこと.これをjsで書き換えることによって今開いてるページの操作ができる.これは`document`単体でもアクセス可能.
document object modelを操作することをDOMという  

#### DOMの操作

HTMLはツリー構造  
jsからいじるときはidを振っていくとやりやすい  

```
document.getElementById(id名)
```

で,HTMLの指定のID名の要素にアクセスできる.例えば,

```
var name_tag = document.getElementById("name");
```

だとか

```
document.getElementById("submit_name").value = "name";
```

でidがnameのタグのvalueを書き換えたりて感じ

- adblockにブロックされる

### <a>タグでPOSTを送る

register.phpに実装  
jsを用いる  
ここを参照にした→[HTMLのAタグでPOSTする方法](https://qiita.com/next1ka2u/items/9736ce2f9c7f3aa69d61)


## サーバーとクライアントで分ける
- まず,機能ごとにどのような物が必要で,それはどのようにサーバーとクライアントで線引きできるのかということを紙に書き出してみる.  
- さらに,サーバーとクライアントでどのようなやり取りを行うかを考える
  - クライアントからリクエストで何を送るのか
  - サーバーからはどんなレスポンスがくるのか.どんなデータ型か.
    - レスポンスはjsonいいかも

クライアントとサーバーの話で分かりやすいやつ  
→[超絶初心者のためのサーバとクライアントの話](https://qiita.com/shuntaro_tamura/items/ae55b99deb9e2a170754)

### 例えばユーザー認証
ユーザー認証を行うときは,サーバーでログイン認証機能を持って,クライアント側でユーザーネームとパスワードの入力を行う.  
この時のリクエストでパスワードとユーザーネームを伴ってレスポンスがくる  
そして,サーバーで認証が行われ認証がマッチすれば,レスポンスとしてトークンが返ってくる.トークンとidでもいい.  

- トークンはハッシュで返す.ユーザー認証みたいなもの.ユーザーで一意に定まるようにする.  


#### なぜトークンか
idで個人を持ってても機能として代わりはないが,それだとidの値を少し変えただけで,他のユーザーのページに入られる恐れがあるため弱い.  
また,ローカルでパスワードを持ってることはありえないため,idとパスワードの組み合わせで持って置くのはない.そのため,以上のものにとって変わるためのtトークンの発行である.


#### という訳で,まずは,ログイン機能でも実装して行こう

 なんか408エラー出てる...
