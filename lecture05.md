## 構築の流れ  

### EC2構築  
- ローカル環境からSSH接続  
- EC2のパッケージの最新化
````
$ sudo yum update  
````
- 関連するパッケージのインストール  
````
$ sudo yum install \
git make gcc-c++ patch \
openssl-devel \
libyaml-devel libffi-devel libicu-devel \
libxml2 libxslt libxml2-devel libxslt-devel \
zlib-devel readline-devel \`
mysql mysql-server mysql-devel \
ImageMagick ImageMagick-devel \
epel-releas
````
- renvをインストール  
````
$ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
````
- renvをどこからでも呼び出せるように環境設定をする  
````
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile  
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile  
$ exec $SHELL -l  
````
- version指定のためruby-buildを設定する 
````
$ git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build  
$ rbenv rehash  
````
- Rubyのversionをreadme指定の3.2.3にする  
````
$ rbenv install 3.2.3 --verbose  
$ rbenv global 3.2.3  
$ rbenv rehash  
$ ruby -v  
````
- bundlerインストール  
````
$ gem install bundler -v 2.3.14  
$ bundler -v  
````
- railsインストール  
````
gem install rails -v 7.1.3.2  
````
- nvmをインストール  
````
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash  
````
- nvmを有効化  
````
$ . ~/.nvm/nvm.sh  
- Node(17.9.1)をインストール  
$ nvm install 17.9.1  
````
- yarnのインストール  
````
npm install --global yarn  
````
- インスタンス作成初期からインストールされているMariaDB用パッケージを削除する  
````
$ sudo yum remove mariadb-*
````

- インスタンス作成初期からインストールされているMariaDB用パッケージを削除する。
````
$ sudo yum remove mariadb-*
````

- MySQLのリポジトリをyumに追加
````
$ sudo yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm
````
- MySQLに必要なパッケージ(mysql-community-server)を取得
````
$ sudo yum install --enablerepo=mysql80-community mysql-community-server
````

