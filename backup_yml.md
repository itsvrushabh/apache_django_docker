version: '3.7'
services:
    
    webserver:
      build: .
      ports:
        - 80:80
      volumes:
        - ./base_site:/var/www/base_site
        - ./apache_django.conf:/etc/apache2/sites-available/000-default.conf
        - ./startup.sh:/var/www/startup.sh
      command: server
    mysql:
      image: mysql:latest
      volumes:
        - db_data:/var/lib/mysql
      ports:
        - 33060:3306
      restart: always
      environment:
        MYSQL_ROOT_PASSWORD: Cisco@123
        MYSQL_DATABASE: app1_db
        MYSQL_USER: cisco
        MYSQL_PASSWORD: cisco123
    postgres:
      image: postgres:latest
      container_name: my_postgres
      ports:
        - 54320:5432
      volumes:
        - db_data:/var/lib/postgresql/data
      environment:
        - DEBUG=false
        - DB_USER=cisco
        - DB_PASS=cisco123
        - DB_NAME=app1_db
    memcached:
      image: 'bitnami/memcached:latest'
      ports:
        - "11211:11211"
      environment:
        - MEMCACHED_CACHE_SIZE=128
      #  - MEMCACHED_USERNAME=cisco
      #  - MEMCACHED_PASSWORD=cisco123
volumes:
  db_data: