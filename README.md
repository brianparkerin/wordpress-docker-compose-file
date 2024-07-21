# wordpress-docker-compose-file





```
# create a docker-compose.yml/yaml file:
$ touch docker-compose.yaml

# To Run the project:
$ docker-compose up -d

# To Tear Down:
$ docker-compose down --volumes

# Stop containers:
$ docker-compose stop

# check the containers:
$ docker ps 
$ docker ps -a 

```

```
version: '3.9'

#Containers of Docker or Services
services:

  # Database
  db:
    image: mysql:8.0
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite
      
  # phpmyadmin
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - '8080:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password 
    networks:
      - wpsite
      
  # Wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - '8000:80'
    restart: always
    volumes: 
     - ./:/var/www/html
     - ./:/var/www/html/wp-content/plugins
     - ./:/var/www/html/wp-content/themes
     - ./:/var/www/html/wp-content/uploads
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    networks:
      - wpsite
networks:
  wpsite:
volumes:
  db_data:

```
