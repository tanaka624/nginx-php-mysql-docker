# docker-compose を使った LAMP 環境のひな形

## コレは何？

docker-compose すれば LAMP 環境が作れる的なヤツ。開発環境を想定してコンテンツやPHPスクリプトはホスト側におく想定。本番環境として使ってもない問題程度にセキュアな設定になっているつもり

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

### メール送信サーバー
* php/conf.d/000sendmail.ini の YOUR_SMTP_HOSTNAME に送信可能な SMTP サーバーを指定する
* Gmail とかの認証が必要なメールサーバーの場合は、その下のコメントアウトしてるやつでいけるかも。試してないけど

### ログ出力先

nginx のログ出力先をローカルディレクトリにしておく。エラーがおきてもわからないので。指定した先に nginx というサブディレクトリを事前に掘っておくこと。/var/log とか、開発環境なら /home/user/なんちゃらとか

* docker-compose.yml
  * 「nginx」の「volumes」の your_log_local_path のところ


### ヴァーチャルホストにする場合

* nginx/conf.d/site.conf を別名でコピーして、さっきの要領で your_host_fqdn、your_host_fqdn_access.log、your_project_local_path を書き換える
* 同じように、「root」の /var/www/webroot も適当に変更する
* docker-compose.yml の「php」および「nginx」の「volumes」の webroot も上にあわせて追記する

### SSL サイトにする場合

* nginx/conf.d/site.conf の下の方でコメントアウトされている箇所を参考に。例として Let's Encrypt を使う想定になってる
* HSTS を有効にしたい場合は nginx/conf.d/000security.conf 下の方でコメントアウトされている箇所を参考に。

### MySQL の root パスワード

docker-compose.yml で MYSQL_ROOT_PASSWORD として定義している。変更したければ「secret」を書き換える

## 実行方法

### 初回起動

````
$ docker-compose -p YOUR_PROJECT_NAME up -d
````

### 終了

````
$ docker-compose -p YOUR_PROJECT_NAME stop
````

### 2回目以降

````
$ docker-compose -p YOUR_PROJECT_NAME start
````

## その他

ホスト環境は Debian または Ubuntu であることを想定して、PHP コンテナ (Alpine) の www-data を Debian/Ubuntu とに同じになるように変更している。変に uid とかチェックしていなければ他の環境でも動くとは思う
