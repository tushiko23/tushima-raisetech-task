# ELB(ALB)を追加して動作確認
- ターゲットグループの作成

- インスタンスにチェックして、ターゲットグループ名「raisetech-task5-target-group」を設定。

- EC2に含まれるVPCで設定。
![](../images/target-group-vpc.png)

- インスタンスにチャックして、「保留中として以下を含める」を選択。
![](../images/taegetgroup-touroku.png)
![](../images/targetgroup-horyuu.png)

- ターゲットグループを作成できたことを確認。
![](../images/targetgrpup.png)

- ロードバランサーの作成
- ロードバランサー名「raisetech-task-road-balancer」を、EC2が存在しているVPCとサブネット内に設定。
![](../images/load-balancer-mei.png)

- セキュリティグループとターゲットグループを設定。
 ![](../images/road-balancer2.png)
![](../images/loud-balancer-target.png)

- セキュリティグループのインバウンドとアウトバウンド
![](../images/road-balancer-sgin.png)
![](../images/road-balancer-sgout.png)

- ターゲットグループのヘルスチェックを確認する。
![](../images/tagetgroup-healthy.png)

- ロードバランサーでアクセス
![](../images/load-balancer-config.hosts-2.png)

- config.hostsを設定
```sh
#config/envieonments/development.rbにて設定
config.hosts  << raisetech-task5-road-balancer-1820876518.ap-northeast-1.elb.amazonaws.com
```
![](../images/config.hosts-roadbalancer.png)

- サーバを再起動して、動作確認
![](../images/elb-check-2.png)