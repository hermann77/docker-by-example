
# Starting MariaDB container instance with name 'mariadb'

```docker run --name mariadb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=<MY_PASSWORD> -e MYSQL_ROOT_HOST=% -d mariadb:latest```

```MYSQL_ROOT_PASSWORD=<MY_PASSWORD>```
to connect to DB from host with password

```MYSQL_ROOT_HOST=%```
is needed to connect from host

```-p 3306:3306```
is mandatory to connect to the DB from host

# Login to MariaDB in container
```docker exec -it mariadb mysql -u root -p<MY_PASSWORD> test_database```


# Import DB-dump from host to container database
```docker exec -i mariadb mysql -u root -p<MY_PASSWORD> < test_database.sql```

# Export DB-dump from DB in container
```docker exec -it mariadb mysqldump -u root -p<MY_PASSWORD> test_database > test.sql```
or
```docker exec mariadb sh -c 'exec mysqldump -uroot -p<MY_PASSWORD> test_database' > test_database.sql```

# Login to the containers console
```docker exec -it mariadb bash```

# Show downloaded images (some maybe used in containers, some not)
```docker images```

# Show containers
```docker ps```

# Stop container
```docker stop mariadb```

# Remove container
```docker rm mariadb```

# Show local IP-address of container
```docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mariadb```



# MySQL-Knowledge, some ones related to docker

## Connect to MariaDB in container
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
