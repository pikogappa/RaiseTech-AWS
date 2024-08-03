##  APサーバ
- サーバ名: Puma
- バージョン名: 6.4.2
![03-01-01](https://github.com/pikogappa/RaiseTech-AWS/blob/images/03-01-01_PumaVersion.png)
- サーバ終了時: Errorになる
![03-01-02](https://github.com/pikogappa/RaiseTech-AWS/blob/images/03-01-02_StopPumaScreen.png)
- 再起動後: 表示される
![03-01-03](https://github.com/pikogappa/RaiseTech-AWS/blob/images/03-01-03_ReStartPumaScreen.png)
## DBサーバ
- サーバ名: MySQL
- バージョン名: 8.4.2
![03-02-01](https://github.com/pikogappa/RaiseTech-AWS/blob/images/03-02-01_MySQLVersion.png)
- サーバ終了時: Errorになる
![03-02-02](https://github.com/pikogappa/RaiseTech-AWS/blob/images/03-02-02_StopMySQLCL.png)
![03-02-03](https://github.com/pikogappa/RaiseTech-AWS/blob/images/03-02-03_StopMySQLScreen.png)
- Railsの構成管理ツール名
  - Bunlder:  特定のプロジェクト内でのGemの依存関係を管理し、Versionを固定する
  - RubyGems: ここのGemインストールと管理を行うツール
## 学んだこと、感じたこと
- 知識
  - Cloud9の裏側はEC2なので、性能はEC2自体に依存する
    - t2.microではメモリが1GB程度で、環境構築インストール時のエラーになった
    - micro→smallとすることでメモリ性能を上げられる
    - t2→t3でCPU性能をあげられる
    - t2.small, t3.smallともに無料利用枠から外れるが、利用料金は大きく変わらない→t3.smallで再構築し、環境構築を実施した
  - Webアプリの構成、HTTP技術の基礎、RubyのフレームワークRails
    - 設定さえできれば、簡易にアプリ開発ができる
    - JavaのSpringBootでも感じたが、フレームワークは便利
 - マインドセット
    - 現場のエンジニアもトライ＆エラーを繰り返しており、壁にあたった際の自走力が大事
