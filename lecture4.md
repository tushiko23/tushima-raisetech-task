# 第４回課題
* VPCを作成する
* EC2とRDSの構築する
* EC2とRDSを接続し、正常かを確認する

## VPC環境を構築
![](lecture4/images/VPC-environment-1.png)
1. VPC[vpc-0c78ca0c4905349ab]を作成する
![](lecture4/images/vpc-0c78ca0c4905349ab.png)
2. VPC[vpc-0c78ca0c4905349ab]上にサブネット及びプライベートサブネットを作成
* パブリックサブネット[subnet-06e8c0dfee1d85d36]をアベイラビリティゾーン[ap-northeast-1a]に作成
![](lecture4/images/task4-subnet-public1-ap-northeast-1a.png)
* パブリックサブネット[subnet-0193c9023cdcc83a0]をアベイラビリティゾーン[ap-northeast-1c]に作成
![](lecture4/images/task4-subnet-public2-ap-northwest-1c.png)
* プライベートサブネット[subnet-042519ce548990bb8]をアベイラビリティゾーン[ap-northeast-1a]に作成
![](lecture4/images//task4-subnet-private1-ap-northeast-1a.png)
* プライベートサブネット[subnet-096b2b874c021c394]アベイラビリティゾーン[ap-northeast-1c]に作成
![](lecture4/images/task4-subnet-private2-ap-northwest-1c.png)
インターネットゲートウェイ[igw-093e5553ae22784b6]を作成し、VPC[vpc-0c78ca0c4905349ab]にアタッチ
![](lecture4/images/igw-task4-igw.png)
* VPC[vpc-0c78ca0c4905349ab]上にパブリックサブネットに関連付けるためのルートテーブル[rtb-01c9953b39c560617]を作成。
* ルートとして送信先10.0.0.0/16でターゲットlocal、追加で送信先0.0.0.0/0をインターネットゲートウェイ[igw-093e5553ae22784b6]をルートに設定。
![](lecture4/images/task4-rtb-public.png)
* ルートテーブル[rtb-01c9953b39c560617]をパブリックサブネット[subnet-06e8c0dfee1d85d36]、[subnet-0193c9023cdcc83a0]に関連付け
![](lecture4/images/task4-rtb-public-subnet.png)
![](lecture4/images/VPC-task4-rtb-public-subnet.png)
* VPC[vpc-0c78ca0c4905349ab]上にプライベートサブネットに関連付けるためのルートテーブル[rtb-042ec775cf927977a]を作成。
* ルートとして送信先10.0.0.0/16でターゲットlocalを設定(デフォルト設定のまま)。
![](lecture4/images/task4-rtb-private.png)
* ルートテーブル[rtb-042ec775cf927977a]をプライベートサブネット[subnet-042519ce548990bb8]、[subnet-096b2b874c021c394]に関連付け
![](lecture4/images/task4-rtb-private-subnet.png)
![](lecture4/images/VPC-task4-rtb-subnet-private.png)

## EC2及びRDSを指定されたVPC環境に構築し、接続するためのセキュリティグループを設定し各リソースにアタッチ
※今回はローカルPCは、VPC[vpc-0c78ca0c4905349ab]上のパブリックサブネット[subnet-06e8c0dfee1d85d36]で構築したcloud9のターミナルを使用しec2にSSH接続する。

![](lecture4/images/task4-kouseizu-environment.png)
* VPC[vpc-0c78ca0c4905349ab]上のパブリックサブネット[subnet-06e8c0dfee1d85d36]でcloud9の環境構築[i-0cc4f830311ece574 (aws-cloud9-raisetech-task-4-environment-28e0060b22db41babf50ad130d322c7e]
![](lecture4/images/cloud9-4-22-1.png)

![](lecture4/images/cloud9-kouseizu-task4.png)

セキュリティグループに[sg-087a5003e150284cc=aws-cloud9-raisetech-task-4-environment]を設定し、cloud9のEC2にアタッチ。
![](lecture4/images/cloud9-sg-087a5003e150284cc.png)
sg-087a5003e150284cc=aws-cloud9-raisetech-task-4-environmentの設定
* アウトバウンド[ポート22を接続するec2のセキュリティグループ(sg-0740637188d204c9a=ec2-rds-6に許可]→ec2にssh接続をするため
* アウトバウンド[ポート3306をRDSのセキュリティグループ(sg-0d60f32112e4c9fc7=rds-ec2-6)に許可]→rdsに接続をするため
![](lecture4/images/sg-087a5003e150284cc-out-1.png)

![](lecture4/images/task4-kouseizu-ec2.png)
* VPC[vpc-0c78ca0c4905349ab]上のパブリックサブネット[subnet-06e8c0dfee1d85d36]でEC2[i-0cc74b4aec470a700=MYEC2server]を構築する。
![](lecture4/images/myec2server-sg.png)
* セキュリティグループに[sg-0740637188d204c9a=ec2-rds-6]設定し、EC2にアタッチ。
sg-0740637188d204c9a=ec2-rds-6の設定
* インバウンド[ポート22を接続をするcloud9のセキュリティグループ(sg-087a5003e150284cc=aws-cloud9-raisetech-task-4-environmentに許可]→ec2にSSH接続するため
![](lecture4/images/ec2-rds-6-in.png)
* アウトバウンド[ポート3306をRDSのセキュリティグループ(sg-0d60f32112e4c9fc7=rds-ec2-6に許可]→rdsに接続するため
![](lecture4/images/ec2-rds-6-out.png)

![](lecture4/images/task4-kouseizu-RDS.png)
* VPC[vpc-0c78ca0c4905349ab]上のサブネットグループ[default-vpc-0c78ca0c4905349ab]でRDS[databaseraisetechproject23]を構築する。
![](lecture4/images/rds-databaseraisetechproject23.png)
* セキュリティグループに[sg-0d60f32112e4c9fc7=rds-ec2-6]設定し、RDSにアタッチ。

sg-0d60f32112e4c9fc7=rds-ec2-6の設定
* インバウンド[ポート3306を接続するec2のセキュリティグループ(sg-0740637188d204c9a=ec2-rds-6)に許可]→rdsに接続するため
* ![](lecture4/images/rds-sg-0d60f32112e4c9fc7.png)

## EC2とRDSを接続し、正常かを確認する
* ローカルPC(cloud9で構築したターミナル)からEC2にSSH接続 chmod 400 "cloud9-test.pem"[この接続で使用するキーペア。ホームディレクトリ下のファイルに配置]を実施し、ファイルの権限変更
* ssh -i "使用するキーペア" ユーザ名@ec2のパブリックIPアドレス コマンドを実施。 ssh -i "cloud9-test.pem" ec2-user@ec2-52-195-170-103.ap-northeast-1.compute.amazonaws.com
* ![](lecture4/images//images4-8.png)
* EC2[i-0cc74b4aec470a700=MYEC2server]からRDS[databaseraisetechproject23]に接続 
![](lecture4/images/ec2-computer-resource.png)
* mysql -h [エンドポイント] -P 3306 -u [RDSで設定したマスタユーザ名] -p コマンドを実施
* mysql -h databaseraisetechproject23.cdisayw68dw6.ap-northeast-1.rds.amazonaws.com -P 3306 -u admintushiko23 -p
* show databases;コマンドを実施し、内容を表示できるか確認。
![](lecture4/images/images4-3.png)
![](lecture4/images//images4-4.png)
