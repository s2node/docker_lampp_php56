# DockerでLAMPPを使う基本セット(PHP 5.6で mysql_系() 関数を使える設定

## ディレクトリ構造とファイル構成

* html/
  * ウェブサーバのHTMLの実ファイル置き場

* docker/
  * docker-compose.yml ファイルがある場所。 docker-compose を実行するディレクトリ。docker-compose build ; docker-compose up 

* docker/web/
  * Apache2, PHPのDocekrfile 置き場

* docker/db/
  * MySQLのDocekrfileと，設定ファイルmy.cnfとデータベースの実ファイル場所

* docker/db/mysql_data/
  * MySQLデータベースの実ファイル場所

## docker-compose.yml の設定

```yaml
version: '3'

services:
  web:
    container_name: docker-web
    build: ./web
    ports:
      - '80:80'
    volumes:
      - ../html:/var/www/html
    hostname: localhost
  mysql:
    container_name: docker-mysql
    build: ./db
    volumes:
      - ./db/mysql_data:/var/lib/mysql
    #ports:
    #- '3306:3306'
    hostname: localhost
    restart: always
    environment:
      MYSQL_DATABASE: 'database_name'
      MYSQL_USER: 'database_userid'
      MYSQL_PASSWORD: 'database_userid_password'
      MYSQL_ROOT_PASSWORD: 'mysql_root_password'
      MYSQL_ROOT_HOST: '%'
      TZ: 'Asia/Tokyo'
  phpmyadmin:
    container_name: docker-phpmyadmin
    image: phpmyadmin/phpmyadmin
    ports:
      - '8080:80'
    restart: always
    environment:
      PMA_HOST: 'mysql'
      UPLOAD_LIMIT: '256M'
```

### docker-compose.yml MySQLデータベース設定部分

```yaml
      MYSQL_DATABASE: 'database_name'
      MYSQL_USER: 'database_userid'
      MYSQL_PASSWORD: 'database_userid_password'
      MYSQL_ROOT_PASSWORD: 'mysql_root_password'
```

* MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD が初期設定で作成する，データベース名，そのデータベースへのアクセス可能なユーザIDとそのパスワード
* MYSQL_ROOT_PASSWORDが MySQLサーバのrootのパスワード

### phpMyAdminを見るURL http://localhost:8080/ の8080設定

```yaml
    ports:
      - '8080:80'
```

### phpMyAdminでアップロードできるサイズ上限を256M。なお，LIMIT_MEMORYが512Mのため，最大512Mまで増やすことが可能

```yaml
      UPLOAD_LIMIT: '256M'
```

## docker-compose build でmysqlのビルドが失敗する場合

```bash
sudo docker-compose build mysql
```

とすると，できる（一度buildしていると，通常ユーザでは上書きできないファイルができる。）

### webでPHPへアップロードできるサイズ上限を500Mへ設定。　docker/web/Dockerfile 内にある500Mの部分

```Dockerfile
    && echo "file_uploads = On\n" \
         "memory_limit = 500M\n" \
         "upload_max_filesize = 500M\n" \
         "post_max_size = 500M\n" \
         "max_execution_time = 600\n" \
         > /usr/local/etc/php/conf.d/uploads.ini
```
