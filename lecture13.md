# 第13回課題
## 課題内容
CloudFormationとAnsibleとCircleCIでRailsアプリケーションの自動デプロイを行い,ServerSpecでアプリケーションのデプロイサーバに指定のテストをして成功することを確認する
今回自動化に使用したレポジトリ[]()

今回デプロイする簡単なCRUD処理ができるRailsアプリケーション

![アプリケーションの操作映像はこちら](https://lecture13-evdence-app.s3.ap-northeast-1.amazonaws.com/Screen+recording+2024-12-22+21.25.34.webm)

Railsアプリケーションのソースコード[raisetech-live8-sample-app](https://github.com/yuta-ushijima/raisetech-live8-sample-app)

構成図

ディレクトリ構成

ソースコードをCloneして使用するうえでの準備事項

リソースの準備
1. AWSアカウント
2. CloudFormationとAWSCLI、各リソース(VPCなどのネットワーク,EC2,RDS,S3)にアクセス権限をもつユーザー。
   (今回はAdministrator権限があるIAMユーザを使用します)
3. IAMアクセスキーとIAMアクセスキーIDの作成。
[]()
4. キーペアの作成(マネジメントコンソールで作成。)
5. <任意>:Circleciの有料プランで与えられる"CircleCIのジョブで使用するIPアドレス"
[参考](https://circleci.com/docs/ja/ip-ranges/)

CircleCI内で環境変数の設定とSSHKeyの設定
表
(今回は、無料枠でのデプロイになるのでsgに設定するSSH22番ポートのインバウンドルールのMYIPは0.0.0.0/0とします。現場やプロジェクトでは有料版で与えれるIPを確認して×.×.×.×/32を設定してください)

logの場所
webserver[nginx] error.log: /var/
appserver[puma] raistech-live8-sample-app/log/

実行結果の確認
自動化処理が完了したエビデンス画像　Circleciのジョブ成功画像　つながり
CFn Ansible Serverspec・CFnのスタック処理画像・アプリケーションの表示・画像の保存がS3に転送・画像することを確認

