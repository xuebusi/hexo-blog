---
title: DockerCompose脚本一键部署wordpress
date: 2024-03-21 15:57:06
tags:
---

```yaml
version: "3" 
# Defines which compose version to use
services:
  # Services line define which Docker images to run. In this case, it will be MySQL server and WordPress image.
  docker-mysql:
    image: mysql:latest
    # image: mysql:latest indicates the MySQL database container image from Docker Hub used in this installation.
    restart: always
    ports:
      - "3306:3306"
    volumes:
      ["./wordpress-mysql-data:/var/lib/mysql"]
    environment:
      MYSQL_ROOT_PASSWORD: your_db_passwrod
      MYSQL_DATABASE: wordpress
      MYSQL_USER: your_db_user
      MYSQL_PASSWORD: your_db_passwrod
      # Previous four lines define the main variables needed for the MySQL container to work: database, database username, database user password, and the MySQL root password.
  wordpress:
    depends_on:
      - docker-mysql
    image: wordpress:latest
    restart: always
    # Restart line controls the restart mode, meaning if the container stops running for any reason, it will restart the process immediately.
    ports:
      - "8000:80"
      # The previous line defines the port that the WordPress container will use. After successful installation, the full path will look like this: http://localhost:8000
    environment:
      WORDPRESS_DB_HOST: docker-mysql:3306
      WORDPRESS_DB_USER: your_db_user
      WORDPRESS_DB_PASSWORD: your_db_passwrod
      WORDPRESS_DB_NAME: wordpress
# Similar to MySQL image variables, the last four lines define the main variables needed for the WordPress container to work properly with the MySQL container.
    volumes:
      ["./wordpress-html:/var/www/html"]
volumes:
  mysql: {}
```