
[student@workstation DO180-apps]$ lab manage-storage start

Setting up workstation for the Guided Exercise: Persisting a MySQL Database

 · Checking podman configuration...............................  SUCCESS
 · Check if the directory used by lab is not created...........  SUCCESS


[student@workstation DO180-apps]$ mkdir -vp /home/student/local/mysql
mkdir: created directory '/home/student/local'
mkdir: created directory '/home/student/local/mysql'


[student@workstation DO180-apps]$ sudo semanage fcontext -a \
>  -t container_file_t '/home/student/local/mysql(/.*)?'


[student@workstation DO180-apps]$ ls -ldZ /home/student/local/mysql
drwxrwxr-x. 2 student student unconfined_u:object_r:user_home_t:s0 6 Oct 17 05:22 /home/student/local/mysql


[student@workstation DO180-apps]$ sudo restorecon -R /home/student/local/mysql


[student@workstation DO180-apps]$ ls -ldZ /home/student/local/mysql
drwxrwxr-x. 2 student student unconfined_u:object_r:container_file_t:s0 6 Oct 17 05:22 /home/student/local/mysql


[student@workstation DO180-apps]$ podman unshare chown 27:27 /home/student/local/mysql


[student@workstation DO180-apps]$ ls -ldZ /home/student/local/mysql
drwxrwxr-x. 2 100026 100026 unconfined_u:object_r:container_file_t:s0 6 Oct 17 05:22 /home/student/local/mysql


[student@workstation DO180-apps]$  podman pull registry.redhat.io/rhel8/mysql-80:1
Trying to pull registry.redhat.io/rhel8/mysql-80:1...
Getting image source signatures
Checking if image destination supports signatures
Copying blob 0689084f485b done  
Copying blob 90c453b00547 done  
Copying blob 91db37500a62 done  
Copying blob 7985264c26f7 done  
Copying config b1403464ce done  
Writing manifest to image destination
Storing signatures
b1403464ce5b7a05c33828a381fd2604bc0f56b91b28b45d6c41c9348d3eb320


[student@workstation DO180-apps]$  podman run --name persist-db \
>  -d -v /home/student/local/mysql:/var/lib/mysql/data \
>  -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 \
>  -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 \
>  registry.redhat.io/rhel8/mysql-80:1
1ee6b21374168d4678241009f37e093cac3a590773e9ba3f5f304064a89f3716


[student@workstation DO180-apps]$ podman ps --format="{{.ID}} {{.Names}} {{.Status}}"
1ee6b2137416 persist-db Up 20 seconds ago

[student@workstation DO180-apps]$  podman run --name persist-db \
>  -d -v /home/student/local/mysql:/var/lib/mysql/data \
>  -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 \
>  -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 \
>  registry.redhat.io/rhel8/mysql-80:1
1ee6b21374168d4678241009f37e093cac3a590773e9ba3f5f304064a89f3716


[student@workstation DO180-apps]$ podman ps --format="{{.ID}} {{.Names}} {{.Status}}"
1ee6b2137416 persist-db Up 20 seconds ago


[student@workstation DO180-apps]$ ls -ld /home/student/local/mysql/items
drwxr-x---. 2 100026 100026 6 Oct 17 05:26 /home/student/local/mysql/items


[student@workstation DO180-apps]$ podman unshare ls -ld /home/student/local/mysql/items
drwxr-x---. 2 27 27 6 Oct 17 05:26 /home/student/local/mysql/items


[student@workstation DO180-apps]$ vi mysql_persistent_database.md


[student@workstation DO180-apps]$ lab manage-storage finish


Completing the Guided Exercise: Persisting a MySQL Database

 · Stopping 'persist-db' container.............................  SUCCESS
 · Removing 'persist-db' container.............................  SUCCESS
 · Removing the 'registry.redhat.io/rhel8/mysql-80:1' image....  SUCCESS
 · Removing the /home/student/local/mysql directory............  SUCCESS
 · Removing the fcontext for /home/student/local/mysql.........  SUCCESS

