 docker-compose を使った LAMP 環境のひな形

## コレは何？

docker-compose すれば LAMP 環境が作れる的なヤツ

## 使う前に

以下の点を変更すること

### FQDN

ブラウザからアクセスされる場合の FQDN。www.なんちゃらとか、開発環境なら localhost とか。

* nginx/conf.d/site.conf
  * 「server_name」の your_host_fqdn のところ
  * 「access_log」の your_host_fqdn_access.log のところ

#### プロジェクトのパス

コンテンツやらPHPスクリプトやらを置く場所。/var/www/なんちゃらとか、開発環境なら /home/user/なんちゃらとか。

* docker-compose.yml
  * 「php」の「volumes」の your_project_local_path のところ
  * 「nginx」の「volumes」の your_project_local_path のところ

* nginx/conf.d/site.conf
  * 「location /」の「root」の your_project_local_path のところ

### メール送信サーバー
* php/conf.d/000sendmail.ini の YOUR_SMTP_HOSTNAME に送信可能な SMTP サーバーを指定する
* Gmail とかの認証が必要なメールサーバーの場合は、その下のコメントアウトしてるやつでいけるかも。試してないけど


### マルチドメイン にする場合

* nginx/conf.d/site.conf を別名でコピーして、さっきの要領で your_host_fqdn、your_host_fqdn_access.log、your_project_local_path を書き換える
* docker-compose.yml の「php」および「nginx」の「volumes」を書き加える

### SSL サイトにする場合

* nginx/conf.d/site.conf の下の方でコメントアウトされている箇所を参考に。例として Let's Encrypt を使う想定になってる
* HSTS を有効にしたい場合は nginx/conf.d/000security.conf 下の方でコメントアウトされている箇所を参考に。

### MySQL の root パスワード

docker-compose.yml で MYSQL_ROOT_PASSWORD として定義している。変更したければ「secret」を書き換える

## 実行方法

````
$ docker-compose up -d
````


## その他

ホスト環境は Ubuntu または Debian であることを想定しているけど、変に uid とかチェックしていなければ他の環境でも動くとは思う
