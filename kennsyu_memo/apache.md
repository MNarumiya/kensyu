# Apacheについて
## Apacheの起動・停止・再起動
起動
```
$ sudo apachectl start
```

再起動
```
sudo apachectl restart
```


## .htaccessについて
### .htaccessとは
Webサーバーに「Apache」を用いている場合に、利用できるファイルで,  
サイトの管理者が、「Apache」の各種設定や制御をおこなえるようにしたファイル.  
ディレクトリ単位で設置・設定が可能.

#### URLを書き換える
- mod_rewriteを用いる
  - mod_rewriteはApacheのモジュールなので設定ファイルで有効にする必要がある
- 正規表現の書き方はPerlとほぼ同じ

### mod_rewriteを有効にする
` /opt/local/etc/apache2/httpd.conf`を書き換える  

### ログファイルをかく
書けなかった!w  

試したこと  
- httpd.confに`LogLevel alert rewrite:trace4`をかく

のでkktさんにきく

##### RewriteCond
RewirteCondは、サーバ変数を参照して、それが指定した正規表現のパターンと一致していれば、次の条件を引き続き実行するという指示演算子

```
RewriteCond %{サーバ変数名} 正規表現パターン
```

##### RewriteRule
RewriteRuleはURLの書き換えを行うもの  
```
RewriteRule 正規表現パターン 置換パターン  [フラグ]
```

- パターンの書き方はmod_rewriteの正規表現の書き方
- フラグは省略してもいい

##### [QSA]フラグについて
置換文字列の中でマッチしたものを書き換えるのではなく、そこにクエリー文字列部分を追加するようにする  

> [QSA]フラグが意味を持つのは、書き換え後のURLにQueryStringが存在する場合だけである。デフォルトでは、元のURLのQueryStringは破棄されるが、[QSA]を指定すると結合されるようになる。  
> 以下の例で、 /aaa?var=1 は /replace?var=2 になるが、/bbb?var=1 は /append?var=2&var=1 になる。
> ```
> RewriteRule ^aaa$ /replace?var=2 [L]
RewriteRule ^bbb$ /append?var=2  [L,QSA]
> ```

##### 例
```
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ server/index.php [QSA,L]
</IfModule>
```

- `<IfModule mod_rewrite.c>`...利用環境でmod_rewriteが利用できるかどうかを確認
- `RewriteEngine On`...mod_rewriteの機能を有効化するという記述
- `RewriteBase /`
  - Rewrite処理後にベースとなるURLの指定.
  - この記述がない場合は、htaccessを設置しているディレクトリが自動的に選択.RewriteRuleを相対パスで記述した場合にのみ適用.
  - 特に理由がない場合は、「RewriteBase /」と記述しておく.どのディレクトリに設置を行った場合でも必ずドキュメントルートからのパス.
- `RewriteCond %{REQUEST_FILENAME} !-f`
  - mod_rewriteで使用するif文
  - RewriteCondの次に記述されいている部分「%{REQUEST_FILENAME}」というものが、「!-f」という条件を満たしていれば真
  - %{REQUEST_FILENAME}は環境変数
  - PHPが利用できるサーバーであれば、`var_dump($_SERVER)`や`phpinfo()`という関数を利用して環境変数にどのような値が変数に入っているのか確認も可能
  - 「-f」はモジュールで準備をしてくれている判定ルール
    - 前方に指定されていた変数、今回の場合は%{REQUEST_FILENAME}に入っている物がファイルであるかどうかを判定.
    - ファイルの場合は、真。ファイル以外の場合は、偽
  - 「!」は否定の演算子
  - 「!-f」の場合はファイル以外の場合は、真を。ファイルの場合は偽を返す

- `RewriteCond %{REQUEST_FILENAME} !-f`
  - 「-d」はディレクトリかどうかの検証
    - %{REQUEST_FILENAME}がディレクトリの場合は真を、それ以外の場合は偽
  - !-f」の場合はディレクトリ以外の場合は、真を。ファイルの場合は偽を返す

- `RewriteRule ^ server/index.php [QSA,L]`
  - `^` ...文字の始まり(ここを`.`としても同じ?)
  - `server` ...サーバーのファイル名??
  - `[L]` ...フラグ.これ以降の記述は適用しない.
  - `[QSA]`...元のURLにリクエストがあれば,それを保持したままでクエリより前だけ,URLを変更する

この処理はサーバーにリクエストを送ったときだけ書き換えられる?

### 参考
- [URLを書き換える -Rewrite機能](https://rfs.jp/server/apache/02apache/url_rewrite.html) ...これが一番分かりやすくて情報も濃かった
- [.htaccessとは？.htaccessで、できる事と設定手順についてまとめてみた](https://viral-community.com/blog/htaccess-3065/)
- [.htaccess の書き方（リダイレクト編）](https://qiita.com/shotets/items/1f8f308e008dcb96bf43)
- [【mod_rewrite】 RewriteRuleとは？](http://ysklog.net/mod-rewrite/rewrite-rule.html)
- [勘違いしやすいmod_rewriteの[QSA]フラグの仕様](https://qiita.com/tkykmw/items/c1e2df5e96d9dad3a07a)
- [Apacheのmod_rewriteモジュールの使い方を徹底的に解説](https://oxynotes.com/?p=7392)


## ApacheのHTTPステータスコード
- 304 ...HTTP_NOT_MODIFIED(リクエストを受けアクセスは許可されたが対象の文書は更新されていなかった)
- 404 ...HTTP_NOT_FOUND

### 参考
- [ApacheのHTTPステータスコードが知りたい](http://www.itmedia.co.jp/help/tips/linux/l0466.html)

次はtoken発行
