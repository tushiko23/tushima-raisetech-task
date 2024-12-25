# Railsアプリケーションをテスト駆動開発(TestDrivenDevelopment:TDD)に基づいて、自動デプロイ&自動稼働する

* 下記の構成図のインフラ構成でRailsアプリケーションを自動デプロイ&自動稼働し、ServerSpecにてテストをします。
* 今回使用するRailsアプリケーションのデプロイ時のイメージとソースコードは下記を参照にしてください。

[デプロイ時のRailsアプリケーションイメージ](https://lecture13-evdence-app.s3.ap-northeast-1.amazonaws.com/Screen+recording+2024-12-22+21.25.34.webm)

[ソースコード](https://github.com/yuta-ushijima/raisetech-live8-sample-app)


## 構成図

![](lecture13/images/01_AWS_Architecture_Diagram.png)

## 自動化構成の解説
1. **IaC（Infrastructure as Code)**  →**CloudFormation**

* 役割:Railsアプリケーションをデプロイ&稼働させるインフラストラクチャをAWS内に構築する
* 選定理由: 
  1. UIに優れている(GUI→AWSコンソールが使える) 
  2. スタック・リソースの状態管理がAWSで自動管理(AmazonS3のバケット内に保存される)
  3. 排他制御が容易。(作成時、CREATE_IN_PROGRESSとなり、ここでは変更不可になる)
  4. 適用失敗時にロールバック機能ある。(更新中のエラーや問題が影響を与えることなく、環境が安定した状態を維持できる)
* 類似サービス: Terraform, Pulumi など

2. **構成管理(プロビジョニング)ツール** → **Ansible**

* 役割: ターゲットノード(今回はEC2)にRailsアプリケーションのデプロイ&稼働に必要な環境構築、設定や変更を加える。
* 選定理由: 
  1. 非エージェント型のツールで,エージェントと呼ばれる通信を行う為のモジュールを事前に導入が不要である
  2. YAMLファイルが理解できれば実装できる
* 類似サービス: Chef、Puppet など
3. **CI/CDツール(「Continuous Integration/Continuous Delivery)** → **CircleCI**
* 役割:  ソフトウェアの開発プロセス(今回は,CloudFormation,Ansible,ServerSpecの実行及び更新)において、コードの変更を常にテストし、自動で本番環境に適用する。
* 選定理由:
  1. 環境構築のコストが低く、手軽に導入できる
  2. 無料枠が用意されていて、公式ドキュメントやUIが充実している
* 類似サービス: GitHubActions AWSCodeBuild Jenkins など

## インフラ構成の解説
