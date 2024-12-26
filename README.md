# Railsアプリケーションを CloudFormation Ansible CircleCI を用いて、自動デプロイ&自動稼働し、ServerSpecでインフラ環境を自動テストをする

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

[Buildupでの日々の学習記録](https://app.build-up.info/enterprises/bEDI6AXZ/portfolio/bQO5AjMgoTd)

#### オンラインプログラミングスクールRaiseTechでの学習記録

|講義|学習内容|課題内容と記録|備考|
|---|-----|-----|-----|
|第1回|学習のマインドセット<br>AWSアカウント作成<br>IAMユーザの作成・推薦設定<br>Cloud9の作成|Cloud9環境下のRubyにて"HelloWorld"の出力<br>※リポジトリにはなし|AdministratorAccess 権限の IAM ユーザーを作成→以後の課題ではこのIAMユーザを使用します|
|第2回|バージョン管理システム "github"の使い方<br>MarkDown記法|Githubにて、Pull Request の発行と完了報告<br>[lecture2.md](./lecture2.md)|学習記録用リポジトリは今後、[tushima-raisetech-task](https://github.com/tushiko23/tushima-raisetech-task)に記録します|
|第3回|Webアプリケーションが稼働する仕組み<br>APPサーバ・DBサーバ<br>構成管理ツール|Cloud9環境でRailsアプリケーションを手動構築でデプロイ<br>[lecture3.md](./lecture3-images.md)||
|第4回|IAMでの権限管理<br>マネジメントコンソールにてAWS環境手動構築|VPC・サブネットをはじめとするネットワークの構築<br>キーペア・EC2・RDSの作成<br>SSH接続・EC2からRDSに接続確認<br>[lecture4.md](./lecture-4-modify.md)|今回は、Cloud9環境下からEC2にSSH接続していますが、第5回以降はローカルPCからSSH接続します|
|第5回|EC2にRailsアプリケーションをデプロイ<br>ELBを用いた冗長化・負荷分散<br>S3の役割<br>AWS構成図の作成|EC2にRailsアプリケーションのデプロイ<br>ALB構成を加えて冗長化してデプロイ<br>S3に画像の保存先を変更<br>[lecture5.md](./lecture5.md)|ここからCloud9を使用せずローカルPCを使用|
|第6回|AWSでの証跡、ロギングツール<br>AWSでの監視、通知ツール<br>AWSでのコスト管理ツール|AWSを利用した日の記録をCloudTrailのイベントから抽出<br>CloudWatchアラームにて、ALBのアラーム設定し、メール通知する<br>AWS Pricing CalculatorにてAWS利用料の見積の作成と,AWS brillingを用いて現在の利用料を報告する<br>[lecture6](./lecture6.md)||
|第7回|AWSでのセキュリティ対策|第4・5回で作成した環境で考えられる脆弱性をまとめる<br>[lecture7](./lecture7.md)||
|第8回|ライブコーディング第4回〜第5回①|課題なし 第4回 第5回の復習||
|第9回|ライブコーディング第4回〜第5回②|課題なし 第4回 第5回の復習||
|第10回|インフラ自動化<br>IaCツールCloudFormationの使用|CloudFormationにて、第5回の環境構築<br>(アプリケーションの実装はなし)|同じIaCツールTerraformでの実装は下記参照|
|第11回|テスト駆動開発(TestDrivenDevelopment:TDD)<br>ServerSpecにてインフラ環境を自動テスト|ServerSpecにて第10回で構築したインフラ環境に自動テストを導入して成功させる<br>[lecture11](./lecture11.md)|10回課題で構築したEC2内で実行。SSH接続を使用せずlocalhostでの実行。|
|第12回|IaCツールTerraformの解説<br>DevOpsの考え方について<br>CI/CDツールの使用|CI/CDツールCircleCIを導入する<br>第10回課題内のCloudFormationのテンプレートに対して、cfn-lintを実行するジョブを組み込み成功させる<br>[lecture12](./lecture12.md)|cfn-lintで検出した警告とEOFエラーを解消するためテンプレートを一部修正しています|
|第13回|構成管理(プロビジョニング)ツール<br>Ansibleの導入<br>CI/CDツールCircleCIとの連携|CI/CDツールCircleCI内にCloudFormation,Ansible,ServerSpecのジョブを組み込み,Railsアプリケーションを自動デプロイ&自動稼働とインフラ環境の自動テストを成功させる<br>[lecture13](./lecture13.md)|Ansibleのみでの実行時のコントロールノードはAmazonlinux2022をOSとするEC2を利用。<br>ServerSpecはSSH接続。|
|第14回|ライブコーディング第13回①|READMEの訂正やこれまでの課題の記録を総括し,ポートフォリオの作成||
|第15回|ライブコーディング第13回②|READMEの訂正やこれまでの課題の記録を総括し,ポートフォリオの作成||
|第16回|現場に出るにあたっての必要な技術と知識<br>現場での立ち振る舞い<br>就職・転職で優位に立つために|課題なし <br>転職活動・現場に出るための準備||

#### その他の学習記録
|学習内容|学習記録|備考|
|-----|-----|-----|
|AWSClIの使用|第4回・第5回環境をAWSCLIで作成<br>[作成したもの](https://github.com/tushiko23/CLI-AWS/tree/modify)||
|IaCツールTerraformの使用|第4回・第5回環境をTerraformにて構築<br>[作成したもの](https://github.com/tushiko23/terraform-AWS/tree/git-lecture)|コードと説明は更新途中なので更新予定|
|SAA取得|取得のエビデンス||
