===== Tomcat with both log & webapps folder mounted
1) docker pull tomcat:7.0

#Assumption a mysql container with name mysqldb is already running & database connection properties would be referring to mysql-container host
2) docker run -it --rm -p 8080:8080 --link mysqldb:mysql-container --name spring3app -v /tomcat/webapps:/usr/local/tomcat/webapps -v  /tomcat/logs:/usr/local/tomcat/logs tomcat:7.0 

3) Also you need to copy Spring3Hibernate.war to /tomcat/webapps
i.e docker-machine scp Spring3HibernateApp.war dockerfile:/tmp


===== Tomcat with webapp included in the image
1.) Build the docker image
docker build -t sandy:tomcat .

#Assumption a mysql container with name mysqldb is already running & database connection properties would be referring to mysql-container host
2.)  docker run -it --rm -p 8080:8080 --link mysqldb:mysql-container --name spring3app -v  /tomcat/logs:/usr/local/tomcat/logs sandy:tomcat

#Various inspection technique
docker inspect -f "{{ .Mounts }}" spring3app
docker inspect -f "{{ .Volumes }}” spring3app
docker inspect -f "{{ .NetworkSettings.IPAddress }}" spring3app
