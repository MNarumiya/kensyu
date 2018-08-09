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
