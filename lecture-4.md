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

* お世話になっております。セキュリティグループに関して、整理しましたので確認よろしくお願いします！
![](lecture4-1/task4-kouseizu-2.png)
* cloud9のEC2のセキュリティグループ　i-0cc4f830311ece574
![](lecture4-1/cloud9-4-22-1.png)
![](lecture4-1/cloud9-4-22-2.png)
インバウンド　なし
アウトバウンド　
ポート22　sg-0740637188d204c9a　ec2-rds-6 [接続するEC2のセキュリティグループを付与]
ポート3306  sg-0d60f32112e4c9fc7 rds-ec2-6 [接続するRDSのセキュリティグループを付与]
* EC2のセキュリティグループ　i-0cc74b4aec470a700 (MYEC2server)
![](lecture4-1/task4-4-22-2.png)
![](lecture4-1/task4-4-22-1.png)
インバウンド 
ポート22　sg-087a5003e150284cc 　[SSH接続に使用したcloud9環境のセキュリティグループを付与]
アウトバウンド　ポート　3306　sg-0d60f32112e4c9fc7 rds-ec2-6　[接続するRDSのセキュリティグループを付与]
* RDSのセキュリティグループ　databaseraisetechproject23
![](lecture4-1/images4-4.png)
![](lecture4-1/RDS-4-22-1.png)
![](lecture4-1/RDS-security-4-22-3.png)
![](lecture4-1/RDS-security-4-22-2.png)
インバウンド　ポート3306  sg-0740637188d204c9a ec2-rds-6 [接続するEC2のセキュリティグループを付与]
アウトバウンド　なし
セキュリティグループ：raisetechSecuringRDSintheCloud、rds-ec2-7、rds-ec2-8に重複分あったため、インバウンド・アウトバウンドなしで訂正しています。

お世話になっております。セキュリティグループ再度修正しましたのでよろしくお願いします。
* EC2のセキュリティグループ　i-0cc74b4aec470a700 (MYEC2server)
![](lecture4-1/ec2-4-22-4.png)
![](lecture4-1/ec2-4-22-6.png)
* RDSのセキュリティグループ　databaseraisetechproject23
![](lecture4-1/rds-4-22-5.png)
![](lecture4-1/rds-4-22-6.png)
![](lecture4-1/4-22-8.png)
![](lecture4-1/4-22-7.png)

ルートテーブルのエビデンス画像の件、大変失礼致しました。こちらになります。
![](lecture4-1/rds-routetable-1.png)

ルートテーブルのルートタブのエビデンス画像を添付しましたので、確認お願いします！
![](lecture4-1/4-28-route-tab.png)

お疲れ様です！ルートテーブルの修正をしましたので、確認をお願いします！
![](lecture4-1/4-29-3.png)
![](lecture4-1/4-29-2.png)
![](lecture4-1/4-29-modify.png)


