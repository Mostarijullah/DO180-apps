[student@workstation DO180-apps]$ vi /etc/containers/registries.conf
[student@workstation DO180-apps]$ vi /etc/yum/
pluginconf.d/ protected.d/  vars/         
[student@workstation DO180-apps]$ vi /etc/yum
yum/         yum.conf     yum.repos.d/ 
[student@workstation DO180-apps]$ vi /etc/yum
yum/         yum.conf     yum.repos.d/ 
[student@workstation DO180-apps]$ vi /etc/yum.repos.d/
[student@workstation DO180-apps]$ vi /etc/yum.repos.d/
epel-modular.repo          epel-testing-modular.repo  redhat.repo                
epel.repo                  epel-testing.repo          rhel_dvd.repo              
[student@workstation DO180-apps]$ vi /etc/yum.repos.d/
epel-modular.repo          epel-testing-modular.repo  redhat.repo                
epel.repo                  epel-testing.repo          rhel_dvd.repo              
[student@workstation DO180-apps]$ vi /etc/yum.repos.d/rhel_dvd.repo 
[student@workstation DO180-apps]$ podman login registry.redhat.io -u mossiddi
Password: 
Login Succeeded!
[student@workstation DO180-apps]$ podman images
REPOSITORY  TAG         IMAGE ID    CREATED     SIZE
[student@workstation DO180-apps]$ ps
    PID TTY          TIME CMD
   8858 pts/1    00:00:00 bash
   9153 pts/1    00:00:00 git
   9461 pts/1    00:00:00 ps


[student@workstation DO180-apps]$ podman ps -a
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES
[student@workstation DO180-apps]$  lab container-create start

Setting up workstation for the Guided Exercise: Creating a MySQL database instance

 · Checking podman configuration...............................  SUCCESS
 · Creating create_table.txt file..............................  SUCCESS
[student@workstation DO180-apps]$ $ podman run --name mysql-basic \
>  -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 \
>  -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 \
>  -d registry.redhat.io/rhel8/mysql-80:1
bash: $: command not found...
[student@workstation DO180-apps]$ podman run --name mysql-basic  -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55  -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55  -d registry.redhat.io/rhel8/mysql-80:1
Trying to pull registry.redhat.io/rhel8/mysql-80:1...
Getting image source signatures
Checking if image destination supports signatures
Copying blob 0689084f485b done  
Copying blob 7985264c26f7 done  
Copying blob 90c453b00547 done  
Copying blob 91db37500a62 done  
Copying config b1403464ce done  
Writing manifest to image destination
Storing signatures
fff2022a22b737b439c58e67d98409149040043436df4746f0add9fc24c2015f
[student@workstation DO180-apps]$ podman ps --format "{{.ID}} {{.Image}} {{.Names}}"
fff2022a22b7 registry.redhat.io/rhel8/mysql-80:1 mysql-basic


[student@workstation DO180-apps]$ podman exec -it mysql-basic /bin/bash
bash-4.4$  mysql -uroot
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.26 Source distribution

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| items              |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql>  USE items;
Database changed
mysql> CREATE TABLE Projects (id int NOT NULL,
    -> -> name varchar(255) DEFAULT NULL,
    -> -> code varchar(255) DEFAULT NULL,
    -> -> PRIMARY KEY (id));^C

^C
mysql> CREATE TABLE Projects (id int NOT NULL,
    ->  name varchar(255) DEFAULT NULL,
    ->  code varchar(255) DEFAULT NULL,
    ->  PRIMARY KEY (id));
Query OK, 0 rows affected (0.14 sec)

mysql> > SHOW TABLE>S ;SHOW TABLES;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '> SHOW TABLE>S' at line 1
+-----------------+
| Tables_in_items |
+-----------------+
| Projects        |
+-----------------+
1 row in set (0.01 sec)

mysql> INSERT INTO Projects (id, name, code) VALUES (1,'DevOps','DO180');
Query OK, 1 row affected (0.05 sec)

mysql>  SELECT * FROM Projects;
+----+--------+-------+
| id | name   | code  |
+----+--------+-------+
|  1 | DevOps | DO180 |
+----+--------+-------+
1 row in set (0.00 sec)

mysql> exit
Bye
bash-4.4$ exit
exit
[student@workstation DO180-apps]$  lab container-create finish

Completing the Guided Exercise: Creating a MySQL database instance

 · Removing "mysql-basic" container............................  SUCCESS
 · Removing "registry.redhat.io/rhel8/mysql-80:1" image........  SUCCESS

