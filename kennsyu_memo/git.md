# Gitについてのメモ
## 1つのローカルリポジトリに複数のリモートリポジトリを対応
### リモートリポジトリの確認
```
$ git remote -v
```

### リモートリポリポジトリの追加
リモートリポリポジトリの追加するコマンドは`git remote add`  
まず1個目,メインリポジトリの登録は次のよう.(originという名前でリモートリポジトリを登録)

```
$ git remote add origin [リモートリポジトリのuri]
```

2個目は名前を変えて登録すれば良い.(hogehogeという名前で登録)

```
$ git remote add hogehoge [リモートリポジトリのuri]
```

### それぞれのリポジトリへPushする
メインリポジトリのmasterブランチへPushは次のよう

```
$ git push origin master
```

hogehogeリポジトリには次のようにpush

```
$ git push origin hogehoge
```

### remoteリポジトリの削除
```
$ git remote rm hogehoge
```


### 参考
- [1つのローカルリポジトリに複数のリモートリポジトリを登録する](https://redamoon.net/log/post/000025.html)

## Gitで差分をとる
### 基本的な
- 基本的は
  ```
  git diff 比較元ブランチ 比較先ブランチ
  ```

- 比較元を指定しない場合は、デフォルトで、「インデックス」を比較元とし、 [インデックス] → [作業ツリー] と比較した際の差分を表示する
  - インデックスとは,ステージ領域のこと

- 最新のコミットと作業ツリーの差分を見る
  ```
  git diff HEAD
  ```

- 指定したコミットと作業ツリーとの差分を見る
  ```
  git diff <commit ID>
  ```

- インデックスとHEADの差分を見る
  - `git diff --cached` または `git diff --cached HEAD`
  - git commit を行うの直前に「あれ？結局何を add したんだっけな〜」と確認するために非常によく使うコマンド

### git diff便利オプション
- ファイル名だけを表示する
  ```
  git diff --name-only
  ```

- 比較するファイルを限定する
  - 実際は「パス」（ディレクトリ名など）を指定して、比較する範囲を限定可能
  - version1
    ```
    // 比較するファイルを限定する
    git diff <コミット名> <コミット名> ―― <ファイル名>
    ```

  - version2(結果はver1とほぼ同じらしい.調べてないけど)
    ```
    // パスやファイル同士を比較する
    git diff <コミット名>:<ファイル名> <コミット名>:<ファイル名>
    ```

- リモートブランチとローカルブランチの差分を比較 ...リモートからpillする時にfetchしてきて,どのくらいローカルとの差があるかを確認する時に使う
  ```
  // リモートを取ってきて・・
  git fetch origin master

  // [ローカル] → [リモート追跡]の差分を見る
  git diff master origin/master
  ```

### 参考
- [git diff を徹底攻略！よく使う便利オプションまとめ、完全版。](http://www-creators.com/archives/755) ...その他,嬉しい情報たくさん

## `git branch -r`について
これはトラッキングリモートを参照している  
のでリモートリポジトリを参照したければまず,

```
git fetch
```

でトラッキングリモートをリモートと同じにする.(トラッキングリモートは常に最新であるべき)  

## `git checkout`について
`git checkout`の機能は二つある.  

1. 作業ブランチを切り替える
2. 指定したコミットもしくはインデックスの状態を作業ツリーに展開する

また前者の場合は2つの意味がある

1. 指定したブランチがローカルリポジトリに存在しているとき  
  ...HEADを<branch>ブランチに移動するだけ
2. 指定したブランチがローカルリポジトリに存在しないでかつ,`origin/<branch>`というブランチがトラッキングリモートブランチとして存在してるとき  
  ...`git checkout -u <branch> origin/<branch>`のショートカット

### `git checkout -b` について
`git checkout -b`の基本は,

```
git checkuot -b チェックアウト先のbranch 派生元のbranch
```

派生元のbranchのブランチを省略した場合は,現在いるブランチが派生元のブランチとなる

### 参考
- [git checkout でブランチ切り替え。仕様とオプションまとめ](http://www-creators.com/archives/1388) ...git checkoutについて分かりやすく書かれていてとても嬉しい
- [git checkout理解してなかった](https://qiita.com/knknkn1162/items/b3af70918770d85bc313)


## `git stash`について
「とあるブランチで作業中だけど、いますぐやりたいことができた。
作業すごく中途半端だからコミットはしたくない。」という時に`git stash`  
stashを使用すると、コミットしていない変更を退避することが可能.  
stashで変更を退避させて、今すぐやりたい作業をして、退避させていた変更を戻して作業を再開することができる.  

### 変更の退避
```
git stash save
```

- saveは省略可能
- コミットしていない変更がある状態で上記のコマンドを実行すると、変更した部分が退避される
  - コミットしていない変更とは、addしたものもaddしていないものもどちらも
  - addした変更は退避させない場合,
    ```
    git stash -k
    ```

    もしくは`git stash --keep-index`

- cleanになって,無事checkoutできる

### 退避した作業の一覧
```
git stash list
```

出力結果:
```
stash@{0}: WIP on test: xxxx
stash@{1}: WIP on commit-sample: xxxx
```

- `stash@{X}`がstashの名前
- `WIP on`のあとはブランチ名
- `xxxx`はstashをしたときのHEADのコミットハッシュとコミットメッセージ

### 退避した作業を戻す
```
git stash apply stash@{0}
```

- `git stash apply stash名`にて退避した作業を元に戻す
- stash名を指定しない場合は、直近に退避された作業を戻す
- 現在チェックアウトしているブランチへ退避した変更が書かれる
  - 変更を退避したときのブランチにも、それ以外のブランチにも戻すことが可能
- addした状態そのままにもどしたいときは、上記のコマンドに--indexオプションを付けて実行
  ```
  git stash apply stash@{0} --index
  ```
- stashを使用して退避した作業を元に戻しても、退避した作業はそのまま残る

### 退避した作業を消す
stashを使用して退避した作業を元に戻しても、退避した作業はそのまま残るため,stashを消化したら消す.

```
git stash drop スタッシュ名
```

いいきに全て消す場合は`git stash clear`

### 退避した作業を元に戻すと同時に、stashのリストから消す
stash apply + dropを一緒にやる

```
git stash pop スタッシュ名
```

### その他
- 作業を退避するときにメッセージを付ける
  ```
  git stash save "stash message"
  ```

- 退避した変更の詳細をみる
  - 変更ファイルの一覧を見る
    ```
    git stash show stash@{0}
    ```

  - 変更内容の詳細もみる
    ```
    git stash show stash@{0} -p
    ```

- 新規追加したファイルも退避させる
  ...新規に追加したファイルをAddしていない状態で退避させる
  ```
  git stash -u
  ```


### 参考
- [【git stash】コミットはせずに変更を退避したいとき](https://qiita.com/chihiro/items/f373873d5c2dfbd03250)
