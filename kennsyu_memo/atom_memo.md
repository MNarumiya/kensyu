# Atomメモ
## テーマ
### シンタックスハイライトとUI
- atom-material-uiとatom-material-syntax
- seti-uiとseti-syntax

結局自分はseti-uiとatom-material-syntaxを使ってる.

## パッケージ
### 参考
- [【Atom】開発が10倍捗る！おすすめ設定・パッケージ・ショートカットをご紹介](https://tech-camp.in/note/technology/1079/)

### ハイライト
- [highlight-line](https://atom.io/packages/highlight-line) ...編集している行をハイライトしてれる
- [highlight-selected](https://atom.io/packages/highlight-selected) ...編集してる変数と同じ変数にハイライトする

### ツールバーを使う
次のPackageをインストール
- [Atom Tool Bar](https://atom.io/packages/tool-bar) ...ツールバーを追加するPackage
- [Flex Tool Bar](https://atom.io/packages/flex-tool-bar) ...json形式でツールバーを設定するPackage
- [minimap-highlight-selected package](https://atom.io/packages/minimap-highlight-selected) ...ハイライトのminimap版

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


#### 参考
- [ツールバーを追加する: Atom Tool Bar(tool-bar) / Flex Tool Bar(flex-tool-bar)](https://rfs.jp/sb/atom-github/atom_package_tool_bar.html)

### インデント範囲を縦線で表示する
- [indent-guide-improved](https://atom.io/packages/indent-guide-improved)

### その他
- [file-icons](https://atom.io/packages/file-icons)

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


## HTML/CSSを使う
- [Emmet](https://atom.io/packages/emmet) ...HTMLやCSSのコーディングをサポートする.省略記法でいける.mdの感覚で使える?
- [autoclose-html](https://atom.io/packages/autoclose-html)

### 参考
- [Emmetの使い方| Atom講座 | [Smart]](https://rfs.jp/sb/atom-github/atom03_emmet_howto.html)

## Atomの設定をGitで管理
最初から設定ファイルをGitで管理するようにしておき、Githubに専用のリポジトリを追加しておくと、スムーズな移行が可能になり、バックアップ対策にも

### 参考
- [Atomの設定をGitで管理する| Atom講座 | [Smart]](https://rfs.jp/sb/atom-github/atom10_github.html)

## Atomエディタが重いときに動作を軽くする
アクティビティモニタをみるとAtom HeiperがCPUを占有しまくってる  
以下を参照  
- [Atomエディタが重いときに動作を軽くする5つの解決方法](https://iwb.jp/atom-editor-setting-faster/)

## PHPを使う
- [linter-php](https://atom.io/packages/linter-php)
  - linter-phpを使うためにはphpのpathをatomのconfig.csonに足してあげなきゃならない
  - phpのpathは`$ which php`で手に入る
  - んで,linter-phpのHPを参考に以下のようにconfig.csonに加えれば良い
    ```
    "linter-php":
    'executablePath': /usr/bin/php
    ```
  - ちなみに,config.csonに保存しようとするとなぜかerrorが出るが無視しても,linterは動いた

- [php-debug](https://atom.io/packages/php-debug)  
  Xdebugと組み合わせてAtom上でのステップ実行を可能するパッケージ
  - Xdebugもインストールが必要 → [https://xdebug.org/](https://xdebug.org/)
  - インストール手順: https://xdebug.org/wizard.php
  - 詳しくはここ: [AtomでXdebugを使ってステップ実行するには](http://murayama.hatenablog.com/entry/2017/04/19/085029)

### 参考
- [PHPプログラマーのためのAtomパッケージ5選](https://itcaret.com/archives/595/)

## Atomの参考にすべき記事
- [GitHub 製エディタ Atom入門](https://qiita.com/k2works/items/1d25888fb3a05058e48f)
