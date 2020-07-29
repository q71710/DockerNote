### Docker 基本指令


### Redmine

https://hub.docker.com/_/redmine

```yml
version: '3.1'

services:
  redmine:
    image: redmine
    container_name: myredmine
    restart: always
    volumes:
      - C:\data\redmine\themes\:/usr/src/redmine/public/themes
      - C:\data\redmine\log\:/usr/src/redmine/log
      - C:\data\redmine\plugins\:/usr/src/redmine/plugins
      - C:\data\redmine\files\:/usr/src/redmine/files
    ports:
      - 8080:3000
    environment:
      REDMINE_DB_MYSQL: db
      REDMINE_DB_PASSWORD: P@ssw0rd
    links:
      - db
     
  db:
    image: mysql:5.6
    container_name: mysql56
    #在 redmine 使用時，遇到文字有中文時會有問題，故加入以下 command 這段
    command: --character-set-server=utf8 --collation-server=utf8_general_ci
    restart: always
    volumes:
      - C:\data\mysqlData\:/var/lib/mysql
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: P@ssw0rd
      MYSQL_DATABASE: redmine

```

### NextCloud

https://hub.docker.com/_/nextcloud

```yml
version: '2'

volumes:
  nextcloud:
  db:

services:
  db:
    image: mariadb
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    restart: always
    volumes:
      - /home/devuser250/datadir/mysql:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=nailiNextCloudPassw@rd
      - MYSQL_PASSWORD=nailiNextCloudPassw@rd
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  app:
    image: nextcloud
    ports:
      - 8081:80
    links:
      - db
    volumes:
      - /home/devuser250/datadir/nextcloud:/var/www/html
    restart: always

```
