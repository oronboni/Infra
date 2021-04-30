# Docker Workshop
Lab 12: Docker Compose

---

## Instructions

 - Create a new folder called "Docker-Compose-wp"
```
$ mkdir Docker-Compose-wp
```

 - Move to the new folder
```
$ cd Docker-Compose-wp
```

 - Create a docker-compose.yml file with the content below:
 
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
       MYSQL_USER: shayki
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
       WORDPRESS_DB_USER: shayki
       WORDPRESS_DB_PASSWORD: wordpress

volumes:
    db_data:
```
 
 - Start all the containers with docker-compose up
```
$ docker-compose up
```
 
 - Wait until docker complete to start the containers
  
 - Open another terminal window
  
 - Check if the containers are running
```
  $ docker ps
```

 - Enter to the mysql conteiner
```
$ docker exec -it {mysql container id} bin/bash
```
 
 - Print the environment variable "MYSQL_USER"
```
$ echo $MYSQL_USER
```

 - What printed?
```
$ shayki
```

 - Exit from the container

 - Navigate to http://localhost:8000/
  
 - You should see the WordPress installation
  
 - Install WordPress according the instructions
  
 - Enjoy your private blog :)
  
 - Go back to terminal and stop the containers
```
$ docker-compose stop
```
 - Remove the volumes
```
$ docker-compose down --volume
```

 - Ensure the cotnainers stopped
```
$ docker-compose ps
```

  
  
  