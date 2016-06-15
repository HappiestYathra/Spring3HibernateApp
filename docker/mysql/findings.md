Dockerization of MySQL

1.) Create docker file for mysql

docker build -t sandy:mysql .

2.) Run Mysql db

##Start docker container without any mounting
docker rm -f mysqldb
docker run —rm -it --name mysqldb  sandy:mysql


##Start docker container with data and logs mounting with automated mounting

docker run -it —rm --name mysqldb -v /var/lib/mysql sandy:mysql
#Run "docker inspect mysqldb” to validate volume mounting
#Run “docker stop mysqldb”

docker run -it —rm --name mysqldb -v /var/lib/mysql -v /var/log sandy:mysql
#Run "docker inspect mysqldb” to validate volume mounting
#Run “docker stop mysqldb”


##Start docker container with data and logs mounting with a specific location in host system

docker run -it --rm --name mysqldb -v /mysql/data:/var/lib/mysql  sandy:mysql
#Run "docker inspect mysqldb” to validate volume mounting
#Run “docker stop mysqldb”

#On host system mysql was not able to create log file as the folder permission was 755 and as a result of which mysql user was not able to write the file, if you do ssh on container and change the permission then /entrypoint.sh mysqld will work
mkdir /mysql/log
chmod 777 /mysql/log

docker run -it --rm --name mysqldb -v /mysql/data:/var/lib/mysql -v /mysql/log:/var/log sandy:mysql
#Run "docker inspect mysqldb” to validate volume mounting
#Run “docker stop mysqldb”

#On host system
touch /mysql/log/mysqld.log
chmod 777 /mysql/log/mysqld.log

docker run -it --rm --name mysqldb -v /mysql/data:/var/lib/mysql -v /mysql/log/mysqld.log:/var/log/mysqld.log sandy:mysql
#Run "docker inspect mysqldb” to validate volume mounting
#Run “docker stop mysqldb”

#When I created a /mysql/log/mysqld.log in host system with 27(mysql) as owner db got up properly
