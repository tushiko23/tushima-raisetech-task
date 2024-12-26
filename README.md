# Railsアプリケーションを CloudFormation Ansible CircleCI を用いて、自動デプロイ&自動稼働し、ServerSpecで自動テストをする

* 下記の構成図のインフラ構成でRailsアプリケーションを自動デプロイ&自動稼働し、ServerSpecにてテストをします。
* 今回使用するRailsアプリケーションのデプロイ時のイメージとソースコードは下記を参照にしてください。

[デプロイ時のRailsアプリケーションイメージ](https://lecture13-evdence-app.s3.ap-northeast-1.amazonaws.com/Screen+recording+2024-12-22+21.25.34.webm)

[アプリケーションのソースコード](https://github.com/yuta-ushijima/raisetech-live8-sample-app)


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

1. EC2
* 役割: 汎用的な仮想サーバで、WebServerであるNginxとApplicationServerであるPumaを稼働させる<br>※ 今回は、簡単な構成でインフラ構成をわかりやすくする+コストを抑えるという理由で1台のインスタンスにNginxとPumaを稼働させています。
* 選定理由: 
  1. 環境構築やデプロイ時の設定を手動構築で行うため、自動化の際にイメージが容易である
  2. 言語の成約がなく、サーバレス独自のルールもないので学習コストがかからず容易に導入が可能である
  3. サーバの起動にかかる時間が短い
  4. サーバの保守や監視を自社で行え、障害が起きた際にも問題を切り分けて対応することができる
* 代替手段: Lambda ECS

2. RDS
* 役割: RailsアプリケーションのDataBaseServerとして使用する
* 選定理由: 
  1. 使用できるDBエンジンバージョンが豊富。
  2. サーバのインストールや設定, パッチの適用やバックアップも自動で行ってくれるので、構築や管理が省力的ですぐに活用できる
* 代替手段: Aurora

3. ALB
* 役割: Railsアプリケーションの冗長化・リクエストの負荷分散
* 選定理由:
  1. HTTP/HTTPS プロトコルに特化したロードバランサーである
  2. HTTP通信の中身を確認することができる(CloudWatchメトリクス・ALBの接続・アクセスログなど)
  3. アクセスするURLに応じて転送先を変えることが可能なため。

4. S3
* 役割:Railsアプリケーションの画像の保存先に設定する
* 選定理由:
  1. 優れた耐久性・可用性で低コストで利用できる(データ喪失はほぼ発生しない)
  2. 権限管理が柔軟にでき,データの公開やアクセス管理を細かく設定できるため。
  3. スケーラブルな容量無制限のストレージであるため

### 学習記録

|講義|学習内容|課題の記録|備考|
|---|-----|-----|-----|
|第1回|学習のマインドセット<br>AWSアカウントの作成<br>IAMの推薦設定<br>Cloud9の作成&Rubyにて"HelloWorld"の出力|リポジトリにはなし|AdministratorAccess 権限の IAM ユーザーを作成→以後の課題ではこのIAMユーザを使用します|
|第2回|バージョン管理システム "github"の使い方<br>MarkDown記法|[lecture2.md](./lecture2.md)|学習記録用リポジトリは今後、[tushima-raisetech-task](https://github.com/tushiko23/tushima-raisetech-task)に記録します|

