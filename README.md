# docker-compose を使った PHP 開発環境

## コレは何？

docker-compose すれば PHP Web アプリの環境を作ることができます。開発環境を想定してコンテンツや PHP スクリプトはホスト側に置く設定としています。本番環境として使っても問題ない程度にセキュアな設定にしています。

## 使う前に

以下の点を変更してください。

### FQDN

ブラウザからアクセスされる場合の FQDN を指定してください。たとえば <www.example.com>、開発環境なら <localhost> など。

* .env ファイル
  * 「NGINX_HOST」の localhost のところ
  * 「ACCESS_LOG」の localhost_access.log の「localhost」のところ

#### プロジェクトのパス

コンテンツや PHP スクリプトを置くホスト側の場所です。本番環境なら /var/www/example、開発環境なら /home/user/example のようになります。

* .env ファイル
  * 「PROJECT_DIR」の ./htdocs のところ

### メール送信サーバー

* php/otherconf/esmtprc を編集します。Gmail 用になっていますので、username と password に自分のものを設定します
* 簡単にする場合、hostname に認証の必要ない MUA を設定して、他は全部コメントアウトするのがいいかもしれません

### ログ出力先

nginx とかのログ出力先をホスト側ディレクトリに変更します。エラーが起きた時ホスト側にログファイルが残ります。指定した先に nginx というサブディレクトリを事前に作ってください。/var/log とか、開発環境なら /home/user/example/log など。

* .env ファイル
  * 「LOG_DIR」の ./log のところ

### ヴァーチャルホストにする場合

* nginx/templates/default.conf.template を別名でコピーして、 ${NGINX_HOST}、${ACCESS_LOG}、${WEB_ROOT} を書き換えます
* docker-compose.yml の「php」および「nginx」の「volumes」の ${PROJECT_DIR}:/var/www/${WEB_ROOT} も上の変更にあわせて追記する

### SSL/TLS サイトにする場合

* nginx/templates/default.conf.template の下の方でコメントアウトされている箇所を参考にしてください。例として Let's Encrypt を使う想定になっています
* docker-compose.yml の「nginx」の「ports」でコメントアウトされている 443 ポートを有効にします
* HSTS を有効にしたい場合は nginx/templates/000security.conf.template 下の方でコメントアウトされている箇所を参考にしてください

### Nginx の設定ファイルを追加したい場合

nginx/templates に *.conf.template ファイルを置くと envsubst で変数を置換されて　/etc/nginx/conf.d に *.conf ファイルが作成されるようになっています。

* nginx/templates に *.conf ファイルを *.conf.template にリネームして保存します

### MySQL の root パスワード

.env で MYSQL_ROOT_PASSWORD として定義しています。変更する場合「secret」を書き換えます。

### MySQL の初期化

MySQLのコンテナを作成するときに読み込ませたいダンプファイルやシェルスクリプトがあれば mysql/initdb にファイルを保存します。

## 実行方法

「YOUR_PROJECT_NAME」は適当に名前をつけて読み替えてください。

### 初回起動

```
$ docker-compose -p YOUR_PROJECT_NAME up -d
```

### 終了

```
$ docker-compose -p YOUR_PROJECT_NAME stop
```


### 2回目以降の起動

```
$ docker-compose -p YOUR_PROJECT_NAME start
```

### MySQL への CLI アクセス

php コンテナの mysql クライアントで日本語入力が使えるようになっているので、php コンテナから CLI を起動してください。

```
$ docker exec -it YOUR_PROJECT_NAME_php_1 mysql -uroot -psecret -hmysql mysql
```

## その他

ホスト環境は Debian または Ubuntu であることを想定しています。変に uid などをチェックしていなければ他の環境でも問題ないと思いますが、世の中にはそういう PHP アプリもあるので注意してください。
