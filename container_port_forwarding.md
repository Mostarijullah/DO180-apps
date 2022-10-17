[student@workstation DO180-apps]$ sudo netstat -ltpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      1424/dnsmasq        
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1049/sshd           
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      1042/cupsd          
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/systemd           
tcp6       0      0 :::22                   :::*                    LISTEN      1049/sshd           
tcp6       0      0 ::1:631                 :::*                    LISTEN      1042/cupsd          
tcp6       0      0 :::111                  :::*                    LISTEN      1/systemd           


[student@workstation DO180-apps]$ lab manage-networking start

Setting up workstation for the Guided Exercise: Loading the Database

 · Checking podman configuration...............................  SUCCESS
 · Creating a host directory for the database container:
   · Adding fcontext policy for /home/student/local/mysql......  SUCCESS
   · Creating the /home/student/local/mysql directory..........  SUCCESS
   · Apply fcontext policy to /home/student/local/mysql........  SUCCESS
   · Change owner of the /home/student/local/mysql to the mysql  SUCCESS
 · Downloading starter project.................................  SUCCESS
 · Downloading solution project................................  SUCCESS

Setup successful. Please proceed with the exercise.





[student@workstation DO180-apps]$ podman run --name mysqldb-port \
>  -d -v /home/student/local/mysql:/var/lib/mysql/data -p 13306:3306 \
>  -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 \
>  -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 \
>  registry.redhat.io/rhel8/mysql-80:1
Trying to pull registry.redhat.io/rhel8/mysql-80:1...
Getting image source signatures
Checking if image destination supports signatures
Copying blob 7985264c26f7 done  
Copying blob 90c453b00547 done  
Copying blob 91db37500a62 done  
Copying blob 0689084f485b done  
Copying config b1403464ce done  
Writing manifest to image destination
Storing signatures
7c48104b21e7aa4912184dcc03448bef7dc83db0c91d2943853e4258ece8dabc


[student@workstation DO180-apps]$ podman ps --format="{{.ID}} {{.Names}} {{.Ports}}"
7c48104b21e7 mysqldb-port 0.0.0.0:13306->3306/tcp


[student@workstation DO180-apps]$ mysql -uuser1 -h 127.0.0.1 -pmypa55 -P13306 \
>  items < /home/student/DO180/labs/manage-networking/db.sql
mysql: [Warning] Using a password on the command line interface can be insecure.


[student@workstation DO180-apps]$ podman exec -it mysqldb-port \
>  mysql -uroot items -e "SELECT * FROM Item"
+----+-------------------+------+
| id | description       | done |
+----+-------------------+------+
|  1 | Pick up newspaper |    0 |
|  2 | Buy groceries     |    1 |
+----+-------------------+------+



[student@workstation DO180-apps]$ mysql -uuser1 -h 127.0.0.1 -pmypa55 -P13306 \
>  items -e "SELECT * FROM Item"
mysql: [Warning] Using a password on the command line interface can be insecure.
+----+-------------------+------+
| id | description       | done |
+----+-------------------+------+
|  1 | Pick up newspaper |    0 |
|  2 | Buy groceries     |    1 |
+----+-------------------+------+



[student@workstation DO180-apps]$  podman exec -it mysqldb-port /bin/bash
bash-4.4$ mysql -uroot items -e "SELECT * FROM Item"
+----+-------------------+------+
| id | description       | done |
+----+-------------------+------+
|  1 | Pick up newspaper |    0 |
|  2 | Buy groceries     |    1 |
+----+-------------------+------+


bash-4.4$ exit
exit



[student@workstation DO180-apps]$ mysql -uuser1 -h 127.0.0.1 -pmypa55 -P13306
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 12
Server version: 8.0.26 Source distribution

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| items              |
+--------------------+
2 rows in set (0.00 sec)


mysql> use items;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed


mysql> SELECT * FROM Item;
+----+-------------------+------+
| id | description       | done |
+----+-------------------+------+
|  1 | Pick up newspaper |    0 |
|  2 | Buy groceries     |    1 |
+----+-------------------+------+
2 rows in set (0.00 sec)


mysql> exit
Bye


[student@workstation DO180-apps]$ vi container_port_forwarding.md


[student@workstation DO180-apps]$ sudo netstat -ltpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      1424/dnsmasq        
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1049/sshd           
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      1042/cupsd          
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/systemd           
tcp6       0      0 :::22                   :::*                    LISTEN      1049/sshd           
tcp6       0      0 ::1:631                 :::*                    LISTEN      1042/cupsd          
tcp6       0      0 :::13306                :::*                    LISTEN      13315/rootlessport  
tcp6       0      0 :::111                  :::*                    LISTEN      1/systemd           
[student@workstation DO180-apps]$ lab manage-networking finish

Completing the Guided Exercise: Loading the Database

 · Stopping the 'mysqldb-port' container.......................  SUCCESS
 · Removing the 'mysqldb-port' container.......................  SUCCESS
 · Removing the 'registry.redhat.io/rhel8/mysql-80:1' image....  SUCCESS
 · Removing the /home/student/local/mysql directory............  SUCCESS
 · Removing the fcontext for /home/student/local/mysql.........  SUCCESS

