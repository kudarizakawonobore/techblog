# 概要
技術ブログ
golang の静的サイトジェネレータである、hugo で構成しています。

# Versions

|product|version|
|-|-|
|go|1.14|
|git|2.24.1.windows.2|
|hugo|0.70.0(extended)|
|os|windows10|

# 環境構築
## Install
公式のインストール手順に順じてください

* [Go](http://golang.jp/install)
* [Git](https://git-scm.com/book/ja/v2/%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%82%8B-Git%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
* [Hugo](https://gohugo.io/getting-started/installing/)

## Download
プロジェクトを Clone したら、 サブモジュールを取得します

```
cd themes/mixedpaper
git submodule update --init --recursive
```

# Build
## Serve
ローカルにホスティングする場合は、下記コマンドを実行すれば、1313ポートにホスティングされます。

```
hugo serve
```

## Output
静的ファイルを生成したい場合は、下記コマンドを実行すると、 ```docs``` ディレクトリにファイルが生成されます。

```
hugo
```