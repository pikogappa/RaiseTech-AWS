## 概要 今回の構築範囲
- NWダイアグラム  
![04-00-01](https://github.com/pikogappa/RaiseTech-AWS/blob/lecture04/images/04/04-00-01_nw-diagram.png)

## 課題1 VPCを作成
- リソース詳細  
  - 4サブネット：2AZ（1a,1c）×2種類（Public,Private）  
  ![04-01-01](/images/04/04-01-01_vpc-overview.png)
  - Publicサブネット: ルートテーブルがローカル+IGWへのルート  
  ![04-01-02](https://github.com/pikogappa/RaiseTech-AWS/blob/lecture04/images/04/04-01-02_vpc-subnet-public-1a.png)
  ![04-01-03](https://github.com/pikogappa/RaiseTech-AWS/blob/lecture04/images/04/04-01-02_vpc-subnet-public-1a.png)
  - Privateサブネット: ルートテーブルがローカルのみ（IGWなし）  

## 課題2 EC2とRDSを構築
### EC2
- リソース詳細
  - インバウンドは22番ポートのSSHでローカル端末からの通信のみを許可
  - アウトバウンドは3306番ポートでのMySQLへの接続を許可
  - VPCは上記で作成したものを利用
![04-02-01](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-02-01_ec2-overview.png)
![04-02-02](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-02-02_ec2-security.png)
![04-02-03](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-02-03_ec2-network.png)
- セキュリティグループ
  - EC2からのRDSのアウトバウンド
![04-03-06](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-03-06_rds-sg-ec2-rds-out.png)

### RDS
- リソース詳細
  - インバウンドでEC2からの通信を許可する
![04-03-01](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-03-01_rds-overview.png)
- サブネットグループ（Private1, Private2）
  - サブネット
![04-03-02](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-03-02_rds-subnetgroup.png)
- セキュリティグループ
  - RDSからEC2のインバウンド
![04-03-05](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-03-05_rds-sg-rds-ec2-in.png)
  - RDSセキュリティグループ
![04-03-07](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-03-07_rds-sg-mysqldb-in.png)
![04-03-08](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-03-08_rds-sg-mysqldb-out.png)
## 課題3 EC2からRDSへ接続
- ターミナル履歴
![04-03-09](https://github.com/pikogappa/RaiseTech-AWS/blob/images/04-03-09_rds-connection-ec2-rds.png)