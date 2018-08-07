# Atomメモ
## シンタックスハイライトとUI
- atom-material-uiとatom-material-syntax
- seti-uiとseti-syntax

## ツールバーを使う
次のPackageをインストール
- [Atom Tool Bar](https://atom.io/packages/tool-bar) ...ツールバーを追加するPackage
- [Flex Tool Bar](https://atom.io/packages/flex-tool-bar) ...json形式でツールバーを設定するPackage

これらをインストールして,ツールバーが表示されるので,ルーツバーの設定アイコンをクリックするとtoolbar.csonが開くので,toolbar.csonを例えば以下のように設定  

```
# This file is used by Flex Tool Bar to create buttons on your Tool Bar.
# For more information how to use this package and create your own buttons,
#   read the documentation on https://atom.io/packages/flex-tool-bar

[
    # githubサイトへ遷移
    {
        type: "url"
        icon: "octoface"
        url: "http://github.com"
        tooltip: "Github Page"
    }
    # project-managerパッケージの起動
    {
        type: "button"
        icon: "repo"
        callback: "project-manager:toggle"
        tooltip: "プロジェクトを開く"
    }
    # remote-ftpパッケージのパネル開閉
    {
        type: "button"
        icon: "cloud-upload"
        callback: "remote-ftp:toggle"
        tooltip: "リモートパネルの開閉"
    }
    # remote-ftpパッケージのリモートからの切断
    {
        type: "button"
        icon: "circle-slash"
        callback: "remote-ftp:disconnect"
        tooltip: "リモートから切断"
    }
    # atom-beautifyパッケージの実行
    {
        type: "button"
        icon: "code"
        callback: "atom-beautify:beautify-editor"
        tooltip: "テキストの整形"
        iconset: "fa"
        mode: "atom-text-editor"
    }
    # markdown-previewのトグル（Markdownファイルでのみ表示）
    {
        type: "button"
        icon: "markdown"
        callback: "markdown-preview:toggle"
        tooltip: "Markdown Preview"
        hide: "!Markdown"
    }
    # 区切り線
    {
        type: "spacer"
    }
  ]

```

そうするとツールバーにアイコンが色々出てくる



### 参考
- [ツールバーを追加する: Atom Tool Bar(tool-bar) / Flex Tool Bar(flex-tool-bar)](https://rfs.jp/sb/atom-github/atom_package_tool_bar.html)

## マークダウンを使う
定番のPackage
- markdown-preview ...デフォで入ってるので,設定で「GitHub Markdownの記法を採用する」みたいなところにチェックを入れる
- markdown-scroll-sync ...スクロールをプレビューと合わせる
- markdown-writer ...mdに対する補完機能など
- [atom-csv-markdown](https://atom.io/packages/atom-csv-markdown) ...csv形式の物をmdの表の形式に整形してくれる
- [markdown-table-editor](https://atom.io/packages/markdown-table-editor) ...mdの表編集を楽にしてくれる
- [document-outline](https://atom.io/packages/document-outline) ...mdの階層構造を表示してくれる

### 参考
- [Atom をMarkdownエディタとして整備](https://qiita.com/kouichi-c-nakamura/items/5b04fb1a127aac8ba3b0)
- [最強のMarkdown編集環境としてのAtom](http://takezoe.hatenablog.com/entry/2017/09/25/083522)
