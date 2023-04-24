# Sphinxの導入方法

## 前提条件

ここではWindows上での使用を前提としている。  
MacOSやLinuxの場合は適宜読み替えを実施すること。  

## Pythonのインストール

Pythonが導入済みの環境であることを前提としている。  
導入済みかはターミナルから

``` powershell
[PowerShell on Windows]

# pythonのバージョン確認
> python -V
```

を実施し、バージョン番号が返ってくればインストール済みとなる。  

未インストールの場合は [公式サイト](https://www.python.org/downloads/) からインストーラーを取得し、実行する。  
複数プロジェクト等により複数のバージョンが混在する場合は [こちらのサイト](https://python-beginner.blog/multiversion/) を参照して切り替える。

## Sphinxのインストール

``` powershell
[PowerShell on Windows]

# Sphinxのインストール
> pip install sphinx
```

## クイックスタート

``` powershell
[PowerShell on Windows]

# クイックスタートの実施
> sphinx-quickstart
```

を実施し、プロジェクトを初期化する。  
途中の選択肢は以下の選択する。

```
Separate source and build directories (y/n) [n]: y
Project name: ドキュメント全体の名称
Author name(s): 作者名
Project release []: 0.0.1 (任意でよい)
Project language [en]: jp
```

## テーマの変更

ここではデフォルトのテーマから「Cloud」テーマに変更する手順を記す。  
テーマは個人の好みであるため、変更しなくてもよいし別のテーマとしてもよい。

### テーマのインストール

``` powershell
[PowerShell on Windows]

# テーマのインストール
> pip install cloud-sptheme
```

### copy.pyの書き換え

copy.pyを以下の様に書き換える

``` diff
- html_theme = 'alabaster'
+ html_theme = 'cloud' 
```

## Markdownの有効化

ReSTだけではなく、Markdownでも記述可能とする。

### プラグインインストール

``` powershell
[PowerShell on Windows]

# プラグインの導入
> pip install myst_parser
```

### copy.pyの書き換え

copy.pyを以下の様に書き換える

``` diff
- extensions = []
+ extensions = ['myst_parser']
+ source_suffix = {
+     '.rst': 'restructuredtext',
+     '.md': 'markdown',
+ }
```

## ビルド

``` powershell
[PowerShell on Windows]

# HTMLの生成
>  .\make.bat html
```


