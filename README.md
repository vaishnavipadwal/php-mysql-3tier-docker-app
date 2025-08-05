# 3-Tier PHP-MySQL Web Application Deployment using Docker Compose


I set up a 3-tier application using:
- Frontend: Nginx (HTML, CSS)
- Backend: PHP 
- Database: MySQL
  
## Steps:
### I Downloaded project folder which include frontend, backend, and database files 
- Means fully worikng project
### Then Create Dockerfile inside backend folder for PHP backend
```
FROM php:8.0-apache
RUN docker-php-ext-install mysqli pdo pdo_mysql
COPY . /var/www/html/
RUN chown -R www-data:www-data /var/www/html
EXPOSE 80
CMD ["apache2-foreground"]
```
###  Then Create Dockerfile inside sql folder for My SQL database
```
FROM mysql:5.7
ENV MYSQL_ROOT_PASSWORD=rootpassword
ENV MYSQL_DATABASE=omrsdb
ENV MYSQL_USER=user
ENV MYSQL_PASSWORD=password
COPY omrsdb.sql /docker-entrypoint-initdb.d/
EXPOSE 3306
```
###  Then Create Docker-compose file in project main folder for My SQL database
-A Docker Compose file (docker-compose.yml) allows us to define and manage multiple containers in a structured way. Instead of running separate docker run commands for each container, we can use Docker Compose to handle everything with a single command.
```
version: '3.8'

services:
  database:
    image: mysql:latest
    container_name: mysql_container
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: omrsdb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - ./SQL File:/docker-entrypoint-initdb.d
      - mysql_data:/var/lib/mysql

  backend:
    build: ./admin
    container_name: php_container
    restart: always
    depends_on:
      - database
    ports:
      - "8080:80"
    volumes:
      - ./admin:/var/www/html

  frontend:
    image: nginx
    container_name: nginx_container
    restart: always
    depends_on:
      - backend
    ports:
      - "80:80"
    volumes:
      - ./css:/usr/share/nginx/html/css
      - ./fonts:/usr/share/nginx/html/fonts
      - ./images:/usr/share/nginx/html/images
      - ./index.php:/usr/share/nginx/html/index.php
    command: [ "nginx", "-g", "daemon off;" ]

volumes:
  mysql_data:
```
### Then push all files in git hub repository
```
git add .
git commit -m "Added Dockerfile"
git push origin main
```
### Then clone  it on ubuntu instance and go with that folder 

### Build and deploy the containers 
```
docker-compose up -d --build
```
### Check running container 
```
docker ps
```
### test the setup 
- Frontend: instance public-IP
- Backend: instance public-IP with 8080 port number
- Database: instance public-IP with 3306
