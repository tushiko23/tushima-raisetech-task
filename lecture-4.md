# 第４回課題
* VPC作成
* EC2とRDSの構築
* EC2とRDSを接続し、正常かを確認する
1. VPC作成　画像
![](lecture4-1/images4-7.png)
2. EC2の構築・セキュリティグループ
![](lecture4-1/ec2-MYec2server.png)
* セキュリティグループ raisetechSecuringEC2intheCloud
![](lecture4-1/ec2-security-group-henkou-1.png)
* インバウンドポート22の(106.154.155.181/32)はマイIP、(sg-0740637188d204c9a)はSSH接続、RDS接続に使用したcloud9のデフォルトのセキュリティグループのIDです。
![](lecture4-1/ec2-lecture4-sgout.png)
* セキュリティグループ ec2-rds-7
![](lecture4-1/ec2-rds-7-sgin.png)
![](lecture4-1/ec2-rds-7-sgout.png)
* アウトバウンドポート3306の(sg-006e2b0f2dc59fa88)は接続したRDSとのセキュリティグループです。

3. RDSの構築・RDSのセキュリティグループ
![](lecture4-1/rds-subnetgroup-2.png)
![](lecture4-1/rds-routetable-1.png)
![](lecture4-1/rds-subnetgroup-3.png)
![](lecture4-1/rds-subnetgroup-1.png)
* セキュリティグループ rds-ec2-6
![](lecture4-1/rds-ec2-6sgin.png)
* インバウンドポート3306の(sg-0740637188d204c9a)はSSH接続、RDS接続に使用したcloud9のデフォルトのセキュリティグループのIDです。
![](lecture4-1/rds-ec2-6-sgout.png)
* セキュリティグループ raisetechSecuringRDSintheCloud
![](lecture4-1/raisetechSecuringRDSintheCloud-in.png)
![](lecture4-1/raisetechSecuringRDSintheCloud-out.png)
* セキュリティグループ  rds-ec2-7
![](lecture4-1/rds-ec2-7-in.png)
* インバウンドポート3306(sg-0da2e00e6469000a4)は接続したec2とのセキュリティグループです。
![](lecture4-1/rds-ec2-7-out.png)
5. ローカルPCからEC2にSSH接続
![](lecture4-1/images4-8.png)
6. EC2からRDSに接続
![](lecture4-1/images4-4.png)
![](lecture4-1/images4-3.png)
