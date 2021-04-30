# Docker Workshop
Lab 11: Wordpress with Docker Compose

---

## Preparations
 - Test that you have docker-compose :

```
$ docker-compose --version
```


## Instructions

 - Create a new folder called "wordpress" and browse to it 
```
$ mkdir wordpress
$ cd wordpress
```

- Create a docker-compose.yml file that starts your WordPress blog and a separate MySQL instance with a volume mount for data persistence:
- replace <YOUR_NAME> with your alias

```
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: <YOUR_NAME>
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: <YOUR_NAME>
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}
```
 
 **Note** The docker volume db_data persists any updates made by WordPress to the database.

 - Start all the containers with docker-compose up in detach mode

```
 $ docker-compose up -d
```
 
 - Wait until docker complete to start the containers
 
 - Check if the containers are running

```
 $ docker ps
```
  - Enter to the mysql container
```
$ docker exec -it {mysql container id} bin/bash
```
 
 - Print the environment variable "MYSQL_USER"
```
$ echo $MYSQL_USER
```

  - What printed?

  - Navigate to http://localhost:8000/
  
  - Check that the website is up. You should see the wordpress installation
  
  - Try to login with the credentials entered on the installation page.
  
  - Stop the containers and remove the volumes 
```
  $ docker-compose down --volumes
```

  - Ensure the containers stopped
```
$ docker ps
```
