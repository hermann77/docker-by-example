
# Starting MariaDB container instance with name '<CONTAINER_NAME>'

```docker run --name <CONTAINER_NAME> -p 3306:3306 -e MYSQL_ROOT_PASSWORD=<MY_PASSWORD> -e MYSQL_ROOT_HOST=% -d mariadb:latest```

```MYSQL_ROOT_PASSWORD=<MY_PASSWORD>```
to connect to DB from host with password

```MYSQL_ROOT_HOST=%```
is needed to connect from host

```-p 3306:3306```
is mandatory to connect to the DB from host

## Login to MariaDB in container
```docker exec -it <CONTAINER_NAME> mysql -u root -p<MY_PASSWORD> test_database```


## Import DB-dump from host to container database
```docker exec -i <CONTAINER_NAME> mysql -u root -p<MY_PASSWORD> < test_database.sql```

## Export DB-dump from DB in container
```docker exec -it <CONTAINER_NAME> mysqldump -u root -p<MY_PASSWORD> test_database > test.sql```
or
```docker exec <CONTAINER_NAME> sh -c 'exec mysqldump -uroot -p<MY_PASSWORD> test_database' > test_database.sql```

## Login to the containers console
```docker exec -it <CONTAINER_NAME> bash```

More info: https://docs.docker.com/engine/reference/run/

## Show downloaded images (some maybe used in containers, some not)
```docker images```

## Show containers (both running and stopped)
```docker ps -a```

## Show logs of container
```docker container logs -f <CONTAINER_NAME>```

## Start|stop container
```docker start|stop <CONTAINER_NAME>```

## Remove container
```docker rm <CONTAINER_NAME>```

## Show local IP-address of container
```docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <CONTAINER_NAME>```



# MySQL-Knowledge, some ones related to docker

## After restart container the created databases still be there. But the GLOBAL MySQL-variables (like long_query_time) are lost.

## Connect to MariaDB from host to container (MySQL/MariaDB-Server is within the container. The connection to the DB comes from the outside (from host))
```mysql -u root -h127.0.0.1 -P3306 --protocol=TCP -p```

## Enable Slow Query Logs
```show variables LIKE '%long%';```
```show variables LIKE '%slow%';```
```SET GLOBAL long_query_time = 3;```
```SET GLOBAL slow_query_log = 'ON';```
```SET GLOBAL slow_query_log_file = '/var/log/mysql/mariadb_slow.log';```
```FLUSH LOGS;```


## Export data with create database
```mysqldump --databases -uroot -p<MY_PASSWORD> test_database > test_database.sql```

## Change password of DB-root-user
```ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<MY_PASSWORD>';```

## If connection from outside doesn't work
At first, we try to set ```-e MYSQL_ROOT_HOST=%``` on creating the container.

If it doesn't work: some docker images ignores environment variable directives (like ```-e MYSQL_ROOT_PASSWORD=<MY_PASSWORD> -e MYSQL_ROOT_HOST=%```), so we have to add the (root) user on other hosts (```%```) manually:

```docker exec -it <YOUR_CONTAINER_NAME> bash```

```mysql -u root -p<MY_PASSWORD>```

```GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '<MY_PASSWORD>';```

```FLUSH PRIVILEGES;```

# Additional information

https://docs.docker.com/engine/reference/run/

https://hub.docker.com/_/mariadb

## How to dump & restore a MariaDB/MySQL database from a docker container
https://davejansen.com/how-to-dump-and-restore-a-mariadb-mysql-database-from-a-docker-container/

## Show mysql queries log 
https://tableplus.com/blog/2018/10/how-to-show-queries-log-in-mysql.html
