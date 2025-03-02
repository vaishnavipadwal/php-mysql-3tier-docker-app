FROM mysql:5.7
ENV MYSQL_ROOT_PASSWORD=rootpassword
ENV MYSQL_DATABASE=omrsdb
ENV MYSQL_USER=user
ENV MYSQL_PASSWORD=password
COPY omrsdb.sql /docker-entrypoint-initdb.d/
EXPOSE 3306
