# Railsアプリケーションを CloudFormation Ansible CircleCI を用いて、自動デプロイ&自動稼働し、ServerSpecでインフラ環境を自動テストをする
## 概要

* 下記の構成図のインフラ構成でRailsアプリケーションをAWS上に自動デプロイ&自動稼働し、ServerSpecにてテストをします。
* 今回使用するRailsアプリケーションのデプロイ時のイメージとソースコードは下記を参照にしてください。

[デプロイ時のRailsアプリケーションイメージ](https://lecture13-evdence-app.s3.ap-northeast-1.amazonaws.com/Screen+recording+2024-12-22+21.25.34.webm)

[アプリケーションのソースコード](https://github.com/yuta-ushijima/raisetech-live8-sample-app)

### 構成図
![](lecture13/images/01_AWS_Architecture_Diagram.png)

## Railsアプリケーションに使用しているライブラリの名称とバージョン

### Ruby 
```
3.2.3
```
### Bundler
```
2.3.14
```
### Rails
```
7.1.3.2
```
### Node
```
v17.9.1
```
### Yarn
```
1.22.19
```

## デプロイ時に使用するサーバーの名称とバージョン
### Nginx (WebServerとして使用, amazon-linux-extrasにてインストール)
```
1.22.1
```
### Puma (ApplicationServerとして使用,gemにてインストール済み,systemdにて自動起動する) 
```
6.4.2
```
### RDS (DataBaseServerとして使用,DBエンジンはMySQL)
```
# DBエンジンのMySQLのバージョン
8.0.40
```

## デプロイ手順
### 手動デプロイする手順は[こちらを参照](lecture5.md)
### 自動デプロイする手順
#### 使用するツール

<details><summary>1. AWS CloudFormation:</summary>
Railsアプリケーションのデプロイに必要なインフラストラクチャをAWSCloud上に生成する。(IaCツール)

</details>

<details><summary>2. AWS CLI:</summary>
CloudFormationのテンプレートのスタック作成及び更新や作成したAWSリソースの動的値を取得する

</details>

<details><summary>3. cfn-lint:</summary> 
CloudFormationのテンプレートで構文エラーやバグがないかをチェックする

</details>

<details><summary>4. CircleCI:</summary> 
コードが変更・更新されたときにRailsアプリケーションを自動デプロイ&稼働させるために必要なパイプラインを自動で行う(CI/CDツール)

</details>

<details><summary>5. Ansible:</summary>  
デプロイするサーバーの環境設定や構築といった事前準備処理を行う

</details>

<details><summary>6. ServerSpec:</summary>
テスト項目を設定し、構築されたインフラ環境を自動テストする

</details>

#### デプロイ手順

 1. CI/CDツールCircleCIにて環境変数の設定をする

    [設定する環境変数はこちら](https://github.com/tushiko23/tushima-raisetech-task/blob/main/lecture13.md?plain=1#L120C1-L120C31)
 2. 


### オンラインプログラミングスクールRaiseTechでの学習記録

|講義|学習内容|課題内容と記録|備考|
|---|-----|-----|-----|
|第1回|学習のマインドセット<br>AWSアカウント作成<br>IAMユーザの作成・推薦設定<br>Cloud9の作成|ルートユーザーとIAMユーザーをMFA有効化<br>BillingをIAMユーザで閲覧できるように設定<br>Cloud9環境下のRubyにて"HelloWorld"の出力<br>※リポジトリにはなし|AdministratorAccess 権限の IAM ユーザーを作成→以後の課題ではこのIAMユーザを使用します|
|第2回|バージョン管理システム "github"の使い方<br>MarkDown記法|Githubにて、Pull Request の発行と完了報告<br>MergeとCollaboratorへの追加の仕方<br>[lecture2.md](./lecture2.md)|学習記録用リポジトリは今後、[tushima-raisetech-task](https://github.com/tushiko23/tushima-raisetech-task)に記録します|
|第3回|Webアプリケーションが稼働する仕組み<br>APPサーバ・DBサーバ<br>構成管理ツール|Cloud9環境でRailsアプリケーションを手動構築でデプロイ<br>[lecture3.md](./lecture3-images.md)||
|第4回|IAMでの権限管理<br>マネジメントコンソールにてAWS環境手動構築|VPC・サブネットをはじめとするネットワークの構築<br>キーペア・EC2・RDSの作成<br>EC2にSSH接続・EC2からRDSに接続確認<br>[lecture4.md](./lecture-4-modify.md)|今回は、Cloud9環境下からEC2にSSH接続していますが、第5回以降はローカルPCからSSH接続します|
|第5回|EC2にRailsアプリケーションをデプロイ<br>ELBを用いた冗長化・負荷分散<br>S3の役割<br>AWS構成図の作成|EC2にRailsアプリケーションのデプロイ<br>ALB構成を加えて冗長化してデプロイ<br>S3に画像の保存先を変更<br>[lecture5.md](./lecture5.md)|ここからCloud9を使用せずローカルPCを使用|
|第6回|AWSでの証跡、ロギングツール<br>AWSでの監視、通知ツール<br>AWSでのコスト管理ツール|AWSを利用した日の記録をCloudTrailのイベントから抽出<br>CloudWatchアラームにて、ALBのアラーム設定し、メール通知する<br>AWS Pricing CalculatorにてAWS利用料の見積の作成と,AWS Billingを用いて現在の利用料を報告する<br>[lecture6](./lecture6.md)||
|第7回|AWSでのセキュリティ対策|第4・5回で作成した環境で考えられる脆弱性をまとめる<br>[lecture7](./lecture7.md)||
|第8回|ライブコーディング第4回〜第5回①|課題なし 第4回 第5回の復習||
|第9回|ライブコーディング第4回〜第5回②|課題なし 第4回 第5回の復習||
|第10回|インフラ自動化<br>IaCツールCloudFormationの使用|CloudFormationにて、第5回の環境構築<br>(アプリケーションの実装はなし)|同じIaCツールTerraformでの実装は下記参照|
|第11回|テスト駆動開発(TestDrivenDevelopment:TDD)<br>ServerSpecにてインフラ環境を自動テスト|ServerSpecにて第10回で構築したインフラ環境に自動テストを導入して成功させる<br>[lecture11](./lecture11.md)|10回課題で構築したEC2内で実行。SSH接続を使用せずlocalhostでの実行。|
|第12回|IaCツールTerraformの解説<br>DevOpsの考え方について<br>CI/CDツールの使用|CI/CDツールCircleCIを導入する<br>第10回課題内のCloudFormationのテンプレートに対して、cfn-lintを実行するジョブを組み込み成功させる<br>[lecture12](./lecture12.md)|cfn-lintで検出した警告とEOFエラーを解消するためテンプレートを一部修正しています|
|第13回|構成管理(プロビジョニング)ツール<br>Ansibleの導入<br>CI/CDツールCircleCIとの連携|CI/CDツールCircleCI内にCloudFormation,Ansible,ServerSpecのジョブを組み込み,Railsアプリケーションを自動デプロイ&自動稼働とインフラ環境の自動テストを成功させる<br>[lecture13](./lecture13.md)|Ansibleのみでの実行時のコントロールノードはAmazonlinux2022をOSとするEC2を利用。<br>ServerSpecはSSH接続。|
|第14回|ライブコーディング第13回①|READMEの訂正やこれまでの課題の記録を総括し,ポートフォリオの作成||
|第15回|ライブコーディング第13回②|READMEの訂正やこれまでの課題の記録を総括し,ポートフォリオの作成||
|第16回|現場に出るにあたっての必要な技術と知識<br>現場での立ち振る舞い<br>就職・転職で優位に立つために|課題なし <br>転職活動・現場に出るための準備|
