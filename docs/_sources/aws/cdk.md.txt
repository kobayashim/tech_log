# AWS CDK(Cloud Development Kit)

## 前提条件

以下の文章は特記事項がないかぎり、開発環境はWindows、ターミナルはPowerShell、使用言語はTypeScriptを前提としている。

## 概要

TypeScriptやPython、JavaなどでAWSリソースを定義し、IaC(Infrastructure as Code)を実現する。  
最終的にAWS CloudFormation を用いてデプロイされる。  

## 必要条件

### node.js v10.13.0以上

#### 確認方法

```powershell
[PowerShell on Windows]

# バージョンの確認
> node -v
```

エラーが発生したり、バージョンが10.13.0未満の場合には以下の手順を参考にインストールを実行する。

#### インストール方法

以下の方法ではnvmを使用し、複数のnode.jsバージョンを切り替えをすることが可能。

##### NVM for Windows

[配布サイト](https://github.com/coreybutler/nvm-windows/releases) から最新版のインストーラーをダウンロードし、実行する。  
インストール後は、Windowsを再起動する。

##### node.js

PowerShellは ___管理者権限___ で実行すること。（右クリックのメニューから「管理者として実行」）。

```powershell
[PowerShell on Windows]

# 最新LTS版をインストール
> nvm install lts

# インストール済みのバージョンを確認(現在の使用バージョンには * が表示される)
> nvm list

# 使用するバージョンの変更
> nvm use x.y.z
```

### AWS CLI

#### 確認方法

```powershell
[PowerShell on Windows]

# バージョンの確認
> aws --version
```

エラーが発生した場合には以下の手順を参考にインストールを実行する。

#### インストール方法

[配布サイト](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html) から最新版のインストーラーをダウンロードし、実行する。  

### CDK

#### 確認方法

```powershell
[PowerShell on Windows]

# バージョンの確認
> cdk --version
```

エラーが発生した場合には以下の手順を参考にインストールを実行する。

#### インストール方法

``` powershell
[PowerShell on Windows]

# ツールキットのインストール
> npm install -g aws-cdk
```

### アクセスキーID / シークレットアクセスキー

[公式ユーザーガイド](https://docs.aws.amazon.com/ja_jp/powershell/latest/userguide/pstools-appendix-sign-up.html) を参考に取得する。  


## 参考

[AWS CDK Intro Workshop](https://cdkworkshop.com/ja/)  
