# Apacheについて
## .htaccessについて
### .htaccessとは
Webサーバーに「Apache」を用いている場合に、利用できるファイルで,  
サイトの管理者が、「Apache」の各種設定や制御をおこなえるようにしたファイル.  
ディレクトリ単位で設置・設定が可能.

#### URLを書き換える
- 正規表現の書き方はPerlとほぼ同じ

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

### 参考
- [URLを書き換える -Rewrite機能](https://rfs.jp/server/apache/02apache/url_rewrite.html) ...これが一番分かりやすくて情報も濃かった
- [.htaccessとは？.htaccessで、できる事と設定手順についてまとめてみた](https://viral-community.com/blog/htaccess-3065/)
- [.htaccess の書き方（リダイレクト編）](https://qiita.com/shotets/items/1f8f308e008dcb96bf43)
- [【mod_rewrite】 RewriteRuleとは？](http://ysklog.net/mod-rewrite/rewrite-rule.html)
