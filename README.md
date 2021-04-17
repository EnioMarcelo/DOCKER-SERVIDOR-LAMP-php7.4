# DOCKER - SERVIDOR LAMP
DOCKER - SERVIDOR LAMP - Apache2, PHP 7.4, Mysql 5.7

docker-compose.yml

```bash

version: '3'

services:
  apache:
    image: 'ebuzaneli/php:7.4-apache'
    hostname: buzza-php
    container_name: buzza-php
    restart: always
    ports:
      - '8080:80'
    volumes:
        - ./www:/var/www/html
    depends_on:
      - db
    links:
      - db
    networks:
      buzza-php-network:
        ipv4_address: 10.5.0.2

  db:
    image: ebuzaneli/buzza-server-mysql:5.7
    hostname: buzza-mysql
    container_name: buzza-mysql
    restart: always
    ports:
      - '33306:3306'
    volumes:
        - ./mysql:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=123456
      - MYSQL_DATABASE=buzza
    networks:
      buzza-php-network:
        ipv4_address: 10.5.0.3

  phpmyadmin:
      image: ebuzaneli/buzza-phpmyadmin:latest 
      hostname: buzza-phpmyadmin
      container_name: buzza-phpmyadmin
      links:
         - db
      environment:
         PMA_HOST: db
         PMA_PORT: 3306
      ports:
         - '8082:80' 
      restart: always
      networks:
         buzza-php-network:
            ipv4_address: 10.5.0.4


networks:
  buzza-php-network:
    driver: bridge 
    ipam:
     config:
       - subnet: 10.5.0.0/16
         gateway: 10.5.0.1      
```

docker-compose up -d
