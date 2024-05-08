# 第４回課題
* VPCを作成する
* EC2とRDSの構築する
* EC2とRDSを接続し、正常かを確認する

## VPC環境を構築
![](lecture4-1/VPC-environment-1.png)
1. VPC[vpc-0c78ca0c4905349ab]を作成する
![](lecture4-1/images4-7.png)
2. VPC[vpc-0c78ca0c4905349ab]上にサブネット及びプライベートサブネットを作成
* パブリックサブネット[subnet-06e8c0dfee1d85d36]をアベイラビリティゾーン[ap-northeast-1a]に作成
![](lecture4-1/task4-subnet-public1-ap-northeast-1a.png)
* パブリックサブネット[subnet-0193c9023cdcc83a0]をアベイラビリティゾーン[ap-northeast-1c]に作成
![](lecture4-1/task4-subnet-public2-ap-northwest-1c.png)
* プライベートサブネット[subnet-042519ce548990bb8]をアベイラビリティゾーン[ap-northeast-1a]に作成
![](lecture4-1/task4-subnet-private1-ap-northeast-1a.png)
* プライベートサブネット[subnet-096b2b874c021c394]アベイラビリティゾーン[ap-northeast-1c]に作成
![](lecture4-1/task4-subnet-private2-ap-northwest-1c.png)
インターネットゲートウェイ[igw-093e5553ae22784b6]を作成し、VPC[vpc-0c78ca0c4905349ab]にアタッチ
![](lecture4-1/igw-task4-igw.png)
* VPC[vpc-0c78ca0c4905349ab]上にパブリックサブネットに関連付けるためのルートテーブル[rtb-01c9953b39c560617]を作成。
* ルートとして送信先10.0.0.0/16でターゲットlocal、追加で送信先0.0.0.0/0をインターネットゲートウェイ[igw-093e5553ae22784b6]をルートに設定。
![](lecture4-1/task4-rtb-public.png)
* ルートテーブル[rtb-01c9953b39c560617]をパブリックサブネット[subnet-06e8c0dfee1d85d36]、[subnet-0193c9023cdcc83a0]に関連付け
![](lecture4-1/task4-rtb-public-subnet.png)
![](lecture4-1/VPC-task4-rtb-public-subnet.png)
* VPC[vpc-0c78ca0c4905349ab]上にプライベートサブネットに関連付けるためのルートテーブル[rtb-042ec775cf927977a]を作成。
* ルートとして送信先10.0.0.0/16でターゲットlocalを設定(デフォルト設定のまま)。
![](lecture4-1/task4-rtb-private.png)
* ルートテーブル[rtb-042ec775cf927977a]をプライベートサブネット[subnet-042519ce548990bb8]、[subnet-096b2b874c021c394]に関連付け
![](lecture4-1/task4-rtb-private-subnet.png)
![](lecture4-1/VPC-task4-rtb-subnet-private.png)
