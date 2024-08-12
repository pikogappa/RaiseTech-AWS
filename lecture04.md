## 概要 今回の構築範囲  
- NWダイアグラム  
![04-00-01](/images/04/04-00-01_nw-diagram.png)  
## 課題1 VPCを作成  
- リソース詳細  
  - 4サブネット：2AZ（1a,1c）×2種類（Public,Private）  
  ![04-01-01](/images/04/04-01-01_vpc-overview.png)  
  - Publicサブネット: ルートテーブルがローカル+IGWへのルート  
  ![04-01-02](/images/04/04-01-02_vpc-subnet-public-1a.png)  
  ![04-01-03](/images/04/04-01-03_vpc-subnet-public-1c.png)  
  - Privateサブネット: ルートテーブルがローカルのみ（IGWなし）  
    - subnet-06c1bcc2e085fc2c2  
    ![04-01-04](/images/04/04-01-04_vpc-subnet-private-1a.png)  
    - subnet-05554277601c5401c  
    ![04-01-05](/images/04/04-01-05_vpc-subnet-private-1c.png)  
## 課題2 EC2とRDSを構築  
### EC2  
- リソース詳細  
  - インバウンドは22番ポートのSSHでローカル端末からの通信のみを許可  
  - アウトバウンドは3306番ポートでのMySQLへの接続を許可  
  - VPCは上記で作成したものを利用  
  ![04-02-01](/images/04/04-02-01_ec2-overview.png)  
  ![04-02-02](/images/04/04-02-02_ec2-security.png)  
  ![04-02-03](/images/04/04-02-03_ec2-networking.png)  
- セキュリティグループ  
  - EC2→RDSのアウトバウンド  
  ![04-02-04](/images/04/04-02-04_sg-ec2tords-out.png)  
### RDS  
- リソース詳細  
  - インバウンドでEC2からの通信を許可する  
![04-03-01](/images/04/04-03-01_rds-overview.png)  
- サブネットグループはプライベートで構成（上記VPC参照）  
  - private-1a: subnet-06c1bcc2e085fc2c2
  - private-1c: subnet-05554277601c5401c
  ![04-03-02](/images/04/04-03-02_rds-subnetgroup.png)  
- セキュリティグループ  
  - RDSからEC2のインバウンド  
  ![04-03-05](/images/04/04-03-03_sg-rdstoec2-in.png)  
## 課題3 EC2からRDSへ接続  
- ターミナル履歴  
![04-04-01](/images/04/04-04-01_rds-connection-ec2-rds.png)  