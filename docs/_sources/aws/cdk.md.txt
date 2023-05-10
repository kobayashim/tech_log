# AWS Cloud Development Kit (CDK)

## 前提条件

以下の文章は特記事項がないかぎり、開発環境はWindows、ターミナルはPowerShell、使用言語はTypeScriptを前提としている。

## 概要・特徴

TypeScriptやPython、JavaなどでAWSリソースを定義し、IaC(Infrastructure as Code)を実現する。  
最終的に直接APIを叩いて変更を実施するか、AWS CloudFormation 形式のテンプレートに変換され、それを用いてデプロイされる。  
yaml等で設定ファイルを作成する場合と比較して、条件分岐や繰り返し処理、ユニットテストの実施など一般的にコードが得意とする表現が可能となる。    

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

## 初期化

ここでは作業用ディレクトリを test_dir とした場合の例を説明する。

```powershell
[PowerShell on Windows]

# 作業用ディレクトリの作成と移動
> mkdir test_dir

# 作業用ディレクトリへの移動
> cd test_dir

# 初期化の実施
> cdk init --language typescript
```

## bootstrap

CDKを使用してデプロイを実施するための前準備を実施する。  

```powershell
[PowerShell on Windows]

# bootstrapの実行
> cdk bootstrap --profile [profile]
```

こちらはアカウント毎、リージョン毎に実施する必要がある。

## デプロイ

### 通常のデプロイ

CloudFormation を使用した完全なデプロイを実施する。

```powershell
[PowerShell on Windows]

# デプロイの実行
> cdk deploy --profile [profile]
```

### ホットスワップデプロイ

ホットスワップが可能であれば、CloudFormation ではなく、APIを使用して直接変更を実施する。    
主にアプリケーションのコード変更を反映させたい場合に実施。  
___実稼働環境への使用は推奨されていない。___  

```powershell
[PowerShell on Windows]

# デプロイの実行
> cdk deploy --hotswap --profile [profile]
```

### CDK Watch

cdk.json ファイルの watch 設定で指定されたファイルを監視し、変更があればホットスワップデプロイを実行する。   
完全なデプロイを行う必要がある場合は、通常のデプロイが実施される。
watch 設定は include と exclude があり、エントリーを単一文字列もしくは文字列配列で指定する。  
各エントリーは cdk.json からの相対パスとして解釈される。  
グロブパターンとして * と ** を指定できる。

```powershell
[PowerShell on Windows]

# 監視の実行の実行
> cdk watch --profile [profile]
```

## 差分確認

変更したコードと、現状を比較して差分を表示する。  
事前に変更箇所を確認したい場合に使用する。  

```powershell
[PowerShell on Windows]

# 差分確認の実行
> cdk diff --profile [profile]
```

## クリーンアップ

AWSからリソースの削除を実施する。  
特段の設定がない場合、クリーンアップを実施しても削除されないリソースがあることに留意する。  

```powershell
[PowerShell on Windows]

# クリーンアップの実行
> cdk destroy --profile [profile]
```

## ステージ別対応の実施

コンテキストを使用する。

deploy等の実施する場合に以下の用にオプションを設定することで、スタックに値を渡すことができる。

```cdk deploy --profile [profile] -c stage=dev```

この値はgetContext()メソッドで受け取ることができる。

```
const app = new cdk.App();
const stage = app.node.getContext('stage')
new TutorialCdkStack(app, `TutorialCdkStack-${stage}`, {});
```

スタック内でも```this```で受け取れる。  
```
export class TutorialCdkStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    const stage = this.node.getContext('stage')
```

なお cdk.json にステージ毎の環境差分を記述することが可能。この場合以下の方法で実施する。

cdk.jsonに環境差分を追加する。

``` diff
  {
    "context": {
+     "prd": {
+       "key": "value1"    
+      },     
+     "stg": {
+       "key": "value2"    
+      }, 
+     "dev": {
+       "key": "value3"    
+      }, 
    }
  }
```

tryGetContext()メソッドでステージを取得し、そのステージに紐づくコンテキスを取得する。

```
const stage = app.node.tryGetContext('stage')
const context = app.node.tryGetContext(stage)
const value = context.key
```


## API仕様

[API Reference](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-construct-library.html) を参照。

## 参考

[AWS CDK Intro Workshop](https://cdkworkshop.com/ja/)  
[AWS CDK Reference Documentation](https://docs.aws.amazon.com/cdk/api/v2/)  