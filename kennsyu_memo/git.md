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
