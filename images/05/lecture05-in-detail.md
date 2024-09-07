## 課題の詳細手順  
### ①組み込みサーバー(Puma)で動作確認  
- [Terminal] ローカル環境からSSH接続  
- [Terminal] EC2のパッケージの最新化
````
sudo yum update  
````
- [Terminal] 関連するパッケージのインストール  
````
sudo yum install \
git make gcc-c++ patch \
openssl-devel \
libyaml-devel libffi-devel libicu-devel \
libxml2 libxslt libxml2-devel libxslt-devel \
zlib-devel readline-devel \`
mysql mysql-server mysql-devel \
ImageMagick ImageMagick-devel \
epel-releas
````
- [Terminal] renvをインストール  
````
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
````
- [Terminal] renvの環境設定  
````
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile  
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile  
exec $SHELL -l  
````
- [Terminal] version指定のためruby-buildを設定 
````
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build  
rbenv rehash  
````
- [Terminal] Rubyのversionを指定  
````
rbenv install 3.2.3 --verbose  
rbenv global 3.2.3  
rbenv rehash  
ruby -v  
````
- [Terminal] bundlerインストール  
````
gem install bundler -v 2.3.14  
bundler -v  
````
- [Terminal] railsインストール  
````
gem install rails -v 7.1.3.2  
````
- [Terminal] nvmをインストール  
````
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash  
````
- [Terminal] nvmを有効化  
````
. ~/.nvm/nvm.sh  
- [Terminal] Node(17.9.1)をインストール  
nvm install 17.9.1  
````
- [Terminal] yarnのインストール  
````
npm install --global yarn  
````
- [Terminal] 初期インストールのMariaDB用パッケージを削除  
````
sudo yum remove mariadb-*
````
- [Terminal] MySQLのリポジトリをyumに追加  
````
sudo yum localinstall https://dev.mysql.com/get/mysql84-community-release-el7-1.noarch.rpm
````
- [Terminal] MySQLに必要なパッケージを取得  
````
sudo yum install --enablerepo=mysql80-community mysql-community-server
````
- [Terminal] 関連パッケージを取得  
````
sudo yum install --enablerepo=mysql80-community mysql-community-devel  
````
- [Terminal] インストールされたMySQL関連を出力  
````
yum list installed | grep mysql  
````
````
mysql-community-client.x86_64         8.4.2-1.el7
mysql-community-client-plugins.x86_64 8.4.2-1.el7
mysql-community-common.x86_64         8.4.2-1.el7
mysql-community-devel.x86_64          8.4.2-1.el7
mysql-community-icu-data-files.x86_64 8.4.2-1.el7
mysql-community-libs.x86_64           8.4.2-1.el7
mysql-community-server.x86_64         8.4.2-1.el7
mysql84-community-release.noarch      el7-1
````
- [Terminal] logファイルを作成  
````
sudo touch /var/log/mysqld.log
````
- [Terminal] mysqldを起動  
````
sudo systemctl start mysqld 
````
- [Terminal] musqldの状態を確認  
````
systemctl status mysqld.service
````
````
mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 金 2024-08-30 10:19:29 UTC; 35s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 23376 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 23441 (mysqld)
   Status: "Server is operational"
   CGroup: /system.slice/mysqld.service
           └─23441 /usr/sbin/mysqld
````
- [Terminal] mysqldがインスタンスの起動と同時に起動するように設定  
````
sudo systemctl enable mysqld
````
- [Terminal] database.ymlにRDSのエンドポイント/ユーザー名/パスワードを登録  
````
cp config/database.yml.sample config/database.yml
````
- [Terminal] config/database.ymlを編集  
````
vim config/database.yml
````
````
編集内容
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: RDS作成時に設定したユーザー
  password: RDS作成時に設定したパスワード
  host:     RDSのエンドポイント 

development:
  <<: *default
  database: raisetech_live8_sample_app_development
 
test:
  <<: *default
  database: raisetech_live8_sample_app_test
````
- [Terminal] bin/setup実行前にスワップ領域を追加（メモリ不足回避のため）  
````
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
````
- [Terminal] ブラウザアクセスのためwebpackをインストール  
````
yarn add webpack webpack-cli
````
- [Terminal] サーバーを起動  
````
bin/dev
````
- [AWS] EC2の設定画面でSGに自端末IPレンジからの3000ポートを許可  
- [Browser] ブラウザでURLを入力  
````
http://[EC2のパブリックIP]:3000
````
- [Browser] ブラウザ表示を確認すると、画像表示でvips.so.42エラーが発生  
- [Terminal] 画像処理のためapplication.rbにmini_magickを追加  
````
vim config/application.rb
````
- [Terminal] 再度実行するもImageMagickがないとと出たので再インストール  
````
sudo yum install ImageMagick ImageMagick-devel
````
- サーバーを再度起動し、画像表示を確認  
````
bin/dev
````
### ②組み込みサーバーとUnixSocketを使った動作確認  
- [Terminal] config/puma.rbで3000からUnixSocketに設定  
````
vim config/puma.rb
````
- [Terminal] リッスンの設定を確認
````
rails s
````
- [Terminal] 別ターミナルでEC2上でCurl実行
````
curl --unix-socket /home/ec2-user/raisetech-live8-sample-app/tmp/sockets/puma.sock http://localhost/
````
### ③Nginxの単体起動確認  
- [Terminal] ec2にnginxをインストール  
````
sudo amazon-linux-extras install nginx1
````
- [Terminal] nginxを起動  
````
sudo systemctl start nginx
````
- [AWS] ec2のSGにポート80番を追加
- [Browser] ブラウザからアクセス  
````
http://[EC2のパブリックIP]:80
````
### ④Nginxと組み込みサーバ、UnixSocketを使った動作確認  
- [Terminal] nginx.confを編集
````
sudo vim /etc/nginx/nginx.conf
````
- [Terminal] nginxの構文をチェック  
````
sudo nginx -t
````
````
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
````
- [Terminal] 再起動し、状態を確認  
````
sudo systemctl start nginx
sudo systemctl status nginx
````
````
9月 01 06:27:08 ip-10-0-10-19.ap-northeast-1.compute.internal systemd[1]
9月 01 06:27:08 ip-10-0-10-19.ap-northeast-1.compute.internal nginx[21661]
9月 01 06:27:08 ip-10-0-10-19.ap-northeast-1.compute.internal nginx[21661]
9月 01 06:27:08 ip-10-0-10-19.ap-northeast-1.compute.internal systemd[1]
````
- [Terminal] puma.serviceを/etc/systemd/system直下に複製  
````
sudo cp ~/raisetech-live8-sample-app/samples/puma.service.sample /etc/systemd/system/puma.service
sudo cat /etc/systemd/system/puma.service
````
- [Terminal] pumaを起動  
````
sudo systemctl daemon-reload
sudo systemctl start puma
sudo systemctl status puma
````
- [Browser]ブラウザからパプリックIPでアクセス  
````
http://[EC2のパブリックIP]
````
- [Terminal] 権限を読み取り・書き込み・実行権限を付与  
````
sudo chmod -R 755 /var/lib/nginx
````
### ⑤ELB（ALB）を追加  
- [AWS] ターゲットグループの作成  
![05-05-02](/images/05/05-05-02_target-group.png)  
![05-05-03](/images/05/05-05-03_target.png)  
- [AWS] ロードバランサー(ALB)の作成  
![05-05-04](/images/05/05-05-04_alb.png)  
- [Terminal] Railsのホスト許可のため、ロードバランサーDNS名を追加  
````
sudo vim config/environments/development.rb
````
````
Rails.application.configure do
  # Settings specified here will take precedence over those in config/application.rb.
  config.hosts << [ロードバランサーのDNS名]
  config.hosts << "raisetech-piko-alb-952388516.ap-northeast-1.elb.amazonaws.com"
  # In the development environment your application's code is reloaded any time
````
- [Browser] アクセス確認  
````
http://[ロードバランサーのDNS名]
````
### ⑥S3を追加  
- [AWS] S3バケットを作成  
![05-06-05](/images/05/05-06-05_aws-s3.png)  
- [AWS] IAMロールを作成  
![05-06-06](/images/05/05-06-06_roll-s3fullaccess.png)  
- [AWS] EC2にIAMロールをアタッチ  
![05-06-05](/images/05/05-06-07_ec2roll.png)  
- [Terminal] storage.ymlにバケット名を記述  
````
sudo vim config/storage.yml
````
````
amazon:
service: S3
region: ap-northeast-1
bucket:[バケット名]
````
- [Terminal] 画像の保存先をlocal→amazonに変更  
````
sudo vim config/environments/development.rb
````
````
  # Store uploaded files on the local file system (see config/storage.yml for options).
  config.active_storage.service = :amazon
````
- [Browser] アクセスと画像保存を確認  
- [AWS] 画像の確認  