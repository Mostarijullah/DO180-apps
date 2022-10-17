[student@workstation DO180-apps]$  lab manage-lifecycle start

Setting up workstation for the Guided Excercise: Managing a MySQL Container

 · Checking podman configuration...............................  SUCCESS
 · Setting up labs folder......................................  SUCCESS
 · Downloading starter project.................................  SUCCESS
 · Downloading solution project................................  SUCCESS

Setup successful. Please proceed with the exercise.

[student@workstation DO180-apps]$  podman run --name mysql-db \
>  registry.redhat.io/rhel8/mysql-80:1
Trying to pull registry.redhat.io/rhel8/mysql-80:1...
Getting image source signatures
Checking if image destination supports signatures
Copying blob 90c453b00547 done  
Copying blob 0689084f485b done  
Copying blob 91db37500a62 done  
Copying blob 7985264c26f7 done  
Copying config b1403464ce done  
Writing manifest to image destination
Storing signatures
=> sourcing 20-validate-variables.sh ...
You must either specify the following environment variables:
  MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
  MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
  MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
  MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
Optional Settings:
  MYSQL_LOWER_CASE_TABLE_NAMES (default: 0)
  MYSQL_LOG_QUERIES_ENABLED (default: 0)
  MYSQL_MAX_CONNECTIONS (default: 151)
  MYSQL_FT_MIN_WORD_LEN (default: 4)
  MYSQL_FT_MAX_WORD_LEN (default: 20)
  MYSQL_AIO (default: 1)
  MYSQL_KEY_BUFFER_SIZE (default: 32M or 10% of available memory)
  MYSQL_MAX_ALLOWED_PACKET (default: 200M)
  MYSQL_TABLE_OPEN_CACHE (default: 400)
  MYSQL_SORT_BUFFER_SIZE (default: 256K)
  MYSQL_READ_BUFFER_SIZE (default: 8M or 5% of available memory)
  MYSQL_INNODB_BUFFER_POOL_SIZE (default: 32M or 50% of available memory)
  MYSQL_INNODB_LOG_FILE_SIZE (default: 8M or 15% of available memory)
  MYSQL_INNODB_LOG_BUFFER_SIZE (default: 8M or 15% of available memory)

For more information, see https://github.com/sclorg/mysql-container


[student@workstation DO180-apps]$ podman logs mysql-db
=> sourcing 20-validate-variables.sh ...
You must either specify the following environment variables:
  MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
  MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
  MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
  MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
Optional Settings:
  MYSQL_LOWER_CASE_TABLE_NAMES (default: 0)
  MYSQL_LOG_QUERIES_ENABLED (default: 0)
  MYSQL_MAX_CONNECTIONS (default: 151)
  MYSQL_FT_MIN_WORD_LEN (default: 4)
  MYSQL_FT_MAX_WORD_LEN (default: 20)
  MYSQL_AIO (default: 1)
  MYSQL_KEY_BUFFER_SIZE (default: 32M or 10% of available memory)
  MYSQL_MAX_ALLOWED_PACKET (default: 200M)
  MYSQL_TABLE_OPEN_CACHE (default: 400)
  MYSQL_SORT_BUFFER_SIZE (default: 256K)
  MYSQL_READ_BUFFER_SIZE (default: 8M or 5% of available memory)
  MYSQL_INNODB_BUFFER_POOL_SIZE (default: 32M or 50% of available memory)
  MYSQL_INNODB_LOG_FILE_SIZE (default: 8M or 15% of available memory)
  MYSQL_INNODB_LOG_BUFFER_SIZE (default: 8M or 15% of available memory)

For more information, see https://github.com/sclorg/mysql-container


[student@workstation DO180-apps]$ podman run --name mysql \
>  -d -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 \
>  -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 \
>  registry.redhat.io/rhel8/mysql-80:1
44b03f069d38d98b4d027669f1c6a7deeb26d235ec70d1179850de29ecb147f4


[student@workstation DO180-apps]$ podman ps
CONTAINER ID  IMAGE                                COMMAND     CREATED         STATUS             PORTS       NAMES
44b03f069d38  registry.redhat.io/rhel8/mysql-80:1  run-mysqld  15 seconds ago  Up 15 seconds ago              mysql


[student@workstation DO180-apps]$ podman cp ~/DO180/labs/manage-lifecycle/db.sql mysql:/


[student@workstation DO180-apps]$ podman exec mysql /bin/bash -c \
>  'mysql -uuser1 -pmypa55 items < /db.sql'
mysql: [Warning] Using a password on the command line interface can be insecure.


[student@workstation DO180-apps]$ podman run --name mysql-2 -it \
>  registry.redhat.io/rhel8/mysql-80:1 /bin/bash

bash-4.4$ mysql -uroot
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)

bash-4.4$ exit
exit


[student@workstation DO180-apps]$ podman ps -a
CONTAINER ID  IMAGE                                COMMAND     CREATED         STATUS                     PORTS       NAMES
b152e8390753  registry.redhat.io/rhel8/mysql-80:1  run-mysqld  3 minutes ago   Exited (1) 3 minutes ago               mysql-db
44b03f069d38  registry.redhat.io/rhel8/mysql-80:1  run-mysqld  2 minutes ago   Up 2 minutes ago                       mysql
ebb37425fb15  registry.redhat.io/rhel8/mysql-80:1  /bin/bash   43 seconds ago  Exited (1) 17 seconds ago              mysql-2



[student@workstation DO180-apps]$ podman exec mysql /bin/bash -c \
>  'mysql -uuser1 -pmypa55 -e "select * from items.Projects;"'
id	name	code
1	DevOps	DO180
mysql: [Warning] Using a password on the command line interface can be insecure.


[student@workstation DO180-apps]$ vi managing_mysql_container.md 


[student@workstation DO180-apps]$ lab manage-lifecycle finish

Completing the Guided Excercise: Managing a MySQL Container

 · Stopping 'mysql' container..................................  SUCCESS
 · Stopping 'mysql-2' container................................  SUCCESS
 · Removing 'mysql' container..................................  SUCCESS
 · Removing 'mysql-2' container................................  SUCCESS
 · Removing 'mysql-db' container...............................  SUCCESS
 · Removing 'registry.redhat.io/rhel8/mysql-80:1' image........  SUCCESS
 · Removing the project directory..............................  SUCCESS
 · Removing the solution directory.............................  SUCCESS
 
