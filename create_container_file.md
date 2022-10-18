[student@workstation DO180-apps]$ lab dockerfile-create start

Setting up workstation for the Guided Exercise: Building a basic Apache Container Image

 · Checking podman configuration...............................  SUCCESS
 · Downloading starter project.................................  SUCCESS
 · Downloading solution project................................  SUCCESS

Setup successful. Please proceed with the exercise.

[student@workstation DO180-apps]$ vim ~/DO180/labs/dockerfile-create/Containerfile

[student@workstation DO180-apps]$ cd ~/DO180/labs/dockerfile-create

[student@workstation dockerfile-create]$ podman build --layers=false \
> -t do285/apache .
STEP 1/7: FROM ubi8/ubi:8.6
Resolved "ubi8/ubi" as an alias (/etc/containers/registries.conf.d/001-rhel-shortnames.conf)
Trying to pull registry.access.redhat.com/ubi8/ubi:8.6...
Getting image source signatures
Checking if image destination supports signatures
Copying blob 0689084f485b done  
Copying blob 7985264c26f7 done  
Copying config 1b7b40f4f1 done  
Writing manifest to image destination
Storing signatures
STEP 2/7: MAINTAINER Mostarijullah Siddiquee <mossiddi@in.ibm.com>
STEP 3/7: LABEL description="A custom Apache container based on UBI 8.6"
STEP 4/7: RUN yum install -y httpd && yum clean all
Updating Subscription Management repositories.
Unable to read consumer identity
Subscription Manager is operating in container mode.

This system is not registered with an entitlement server. You can use subscription-manager to register.


Installed:
  apr-1.6.3-12.el8.x86_64                                                       
  apr-util-1.6.1-6.el8.x86_64                                                   
  apr-util-bdb-1.6.1-6.el8.x86_64                                               
  apr-util-openssl-1.6.1-6.el8.x86_64                                           
  httpd-2.4.37-47.module+el8.6.0+15654+427eba2e.2.x86_64                        
  httpd-filesystem-2.4.37-47.module+el8.6.0+15654+427eba2e.2.noarch             
  httpd-tools-2.4.37-47.module+el8.6.0+15654+427eba2e.2.x86_64                  
  mailcap-2.1.48-3.el8.noarch                                                   
  mod_http2-1.15.7-5.module+el8.6.0+13996+01710940.x86_64                       
  redhat-logos-httpd-84.5-1.el8.noarch                                          

Complete!
Updating Subscription Management repositories.
Unable to read consumer identity
Subscription Manager is operating in container mode.

This system is not registered with an entitlement server. You can use subscription-manager to register.

25 files removed
STEP 5/7: RUN echo "Hello from Containerfile" > /var/www/html/index.html
STEP 6/7: EXPOSE 80
STEP 7/7: CMD ["httpd", "-D", "FOREGROUND"]
COMMIT do285/apache
Getting image source signatures
Copying blob db1385a464bb skipped: already exists  
Copying blob 152757e7d372 skipped: already exists  
Copying blob a941df55de6a done  
Copying config f8b5c3a394 done  
Writing manifest to image destination
Storing signatures
--> f8b5c3a3945
Successfully tagged localhost/do285/apache:latest
f8b5c3a3945c289e64aa54e454ab94e5a362f0b83595daf0f08807933692accb

[student@workstation dockerfile-create]$ podman images
REPOSITORY                           TAG         IMAGE ID      CREATED        SIZE
localhost/do285/apache               latest      f8b5c3a3945c  9 seconds ago  249 MB
quay.io/mossiddi/do285-custom-httpd  v1.0        9d3f3ba74b83  18 hours ago   236 MB
registry.access.redhat.com/ubi8/ubi  8.6         1b7b40f4f1ee  6 days ago     227 MB

[student@workstation dockerfile-create]$ podman run --name lab-apache \
>  -d -p 10080:80 do285/apache
0763be72ca877a77c8453586bb30d8ec8e9790c868ea54d07b1511706e64ff59

[student@workstation dockerfile-create]$ podman ps
CONTAINER ID  IMAGE                          COMMAND               CREATED         STATUS             PORTS                  NAMES
0763be72ca87  localhost/do285/apache:latest  httpd -D FOREGROU...  15 seconds ago  Up 16 seconds ago  0.0.0.0:10080->80/tcp  lab-apache

[student@workstation dockerfile-create]$ curl -s 127.0.0.1:10080
Hello from Containerfile

[student@workstation dockerfile-create]$ ls
Containerfile

[student@workstation dockerfile-create]$ cp Containerfile ~/DO
DO180/      DO180-apps/ DO280/      

[student@workstation dockerfile-create]$ cp Containerfile ~/DO180-apps/

[student@workstation dockerfile-create]$ cd ~/DO180-apps/

[student@workstation DO180-apps]$ ls
Containerfile                 DO285-VT.md                  mysql-database.md             php-helloworld
container_port_forwarding.md  example                      mysql_persistent_database.md  README.md
custom_container_image.md     git.md                       nodejs-app                    temps
DO285-VCT-D2.md               managing_mysql_container.md  nodejs-helloworld             todoapp

[student@workstation DO180-apps]$ vi create_container_file.md

