[student@workstation DO180-apps]$ lab image-operations start

Setting up workstation for the GE: Creating a Custom Apache Container Image

 · Checking podman configuration...............................  SUCCESS

[student@workstation DO180-apps]$ podman login quay.io
Username: 
Password: 
Login Succeeded!


[student@workstation DO180-apps]$ podman run -d --name official-httpd \
>  -p 8180:80 quay.io/redhattraining/httpd-parent
Trying to pull quay.io/redhattraining/httpd-parent:latest...
Getting image source signatures
Copying blob 08b8c9fdec44 done  
Copying blob a3ed95caeb02 done  
Copying blob a3ed95caeb02 done  
Copying blob 787f47dbeaac done  
Copying blob a3ed95caeb02 done  
Copying blob 6a5240d60dc4 done  
Copying blob a3ed95caeb02 skipped: already exists  
Copying blob a3ed95caeb02 skipped: already exists  
Copying blob a3ed95caeb02 skipped: already exists  
Copying blob 408208567b9a done  
Writing manifest to image destination
Storing signatures
7013cb61c60154cba1bc1b96eb37e43bfe1e5611c69f5c9d7e7efd4371a41b8e



[student@workstation DO180-apps]$ podman exec -it official-httpd /bin/bash
bash-4.4# echo "DO285 Page" > /var/www/html/do285.html
bash-4.4# exit
exit

[student@workstation DO180-apps]$ curl 127.0.0.1:8180/do285.html
DO285 Page




[student@workstation DO180-apps]$ podman diff official-httpd
C /root
A /root/.bash_history
C /run/httpd
A /run/httpd/cgisock.1
A /run/httpd/httpd.pid
C /tmp
C /var
C /var/log
C /var/log/httpd
A /var/log/httpd/access_log
A /var/log/httpd/error_log
C /var/www
C /var/www/html
A /var/www/html/do285.html


[student@workstation DO180-apps]$ podman stop official-httpd
official-httpd



[student@workstation DO180-apps]$ podman commit -a 'Mostarijullah Siddiquee' \
> official-httpd do285-custom-httpd
Getting image source signatures
Copying blob 24d85c895b6b skipped: already exists  
Copying blob c613b100be16 skipped: already exists  
Copying blob 574bcc187eda skipped: already exists  
Copying blob 7f9108fde4a1 skipped: already exists  
Copying blob d5752c899dd8 done  
Copying config 9d3f3ba74b done  
Writing manifest to image destination
Storing signatures
9d3f3ba74b83148f722acbad512e30b6bd1ad924cfc80786d26685c0b073fe3b



[student@workstation DO180-apps]$ podman images
REPOSITORY                           TAG         IMAGE ID      CREATED         SIZE
localhost/do285-custom-httpd         latest      9d3f3ba74b83  12 seconds ago  236 MB
quay.io/redhattraining/httpd-parent  latest      4346d3cace25  3 years ago     236 MB


[student@workstation DO180-apps]$ source /usr/local/etc/ocp4.config


[student@workstation DO180-apps]$ podman tag do285-custom-httpd \
> quay.io/${RHT_OCP4_QUAY_USER}/do285-custom-httpd:v1.0


[student@workstation DO180-apps]$ podman images
REPOSITORY                           TAG         IMAGE ID      CREATED             SIZE
localhost/do285-custom-httpd         latest      9d3f3ba74b83  About a minute ago  236 MB
quay.io/mossiddi/do285-custom-httpd  v1.0        9d3f3ba74b83  About a minute ago  236 MB
quay.io/redhattraining/httpd-parent  latest      4346d3cace25  3 years ago         236 MB


[student@workstation DO180-apps]$ podman push \
>  quay.io/${RHT_OCP4_QUAY_USER}/do285-custom-httpd:v1.0
Getting image source signatures
Copying blob d5752c899dd8 done  
Copying blob c613b100be16 skipped: already exists  
Copying blob 24d85c895b6b skipped: already exists  
Copying blob 574bcc187eda skipped: already exists  
Copying blob 7f9108fde4a1 skipped: already exists  
Copying config 9d3f3ba74b done  
Writing manifest to image destination
Storing signatures



[student@workstation DO180-apps]$ podman images
REPOSITORY                           TAG         IMAGE ID      CREATED        SIZE
localhost/do285-custom-httpd         latest      9d3f3ba74b83  2 minutes ago  236 MB
quay.io/mossiddi/do285-custom-httpd  v1.0        9d3f3ba74b83  2 minutes ago  236 MB
quay.io/redhattraining/httpd-parent  latest      4346d3cace25  3 years ago    236 MB


[student@workstation DO180-apps]$ podman rmi -a
Untagged: localhost/do285-custom-httpd:latest
Untagged: quay.io/mossiddi/do285-custom-httpd:v1.0
Deleted: 9d3f3ba74b83148f722acbad512e30b6bd1ad924cfc80786d26685c0b073fe3b
Error: Image used by 7013cb61c60154cba1bc1b96eb37e43bfe1e5611c69f5c9d7e7efd4371a41b8e: image is in use by a container


[student@workstation DO180-apps]$ podman rmi -a --force
Untagged: quay.io/redhattraining/httpd-parent:latest
Deleted: 4346d3cace25776120260d3bfadb82dcde503b8bf1d52ccf0a65848eb6852d3b



[student@workstation DO180-apps]$ podman images
REPOSITORY  TAG         IMAGE ID    CREATED     SIZE



[student@workstation DO180-apps]$  podman pull \
>  -q quay.io/${RHT_OCP4_QUAY_USER}/do285-custom-httpd:v1.0
9d3f3ba74b83148f722acbad512e30b6bd1ad924cfc80786d26685c0b073fe3b



[student@workstation DO180-apps]$ podman run -d --name test-httpd -p 8280:80 \
>  ${RHT_OCP4_QUAY_USER}/do285-custom-httpd:v1.0
d2126a6e5eff66b8b8db18c273d22266e117eda1e700e0644a292d92d5e2ec7f



[student@workstation DO180-apps]$ curl http://localhost:8280/do285.html
DO285 Page

[student@workstation DO180-apps]$ vi custom_container_image.md

[student@workstation DO180-apps]$ vi custom_container_image.md
[student@workstation DO180-apps]$ lab image-operations finish

Completing the GE: Creating a Custom Apache Container Image

 · Stopping official-httpd container...........................  SUCCESS
 · Removing official-httpd container...........................  SUCCESS
 · Stopping test-httpd container...............................  SUCCESS
 · Removing test-httpd container...............................  SUCCESS
 · Removing redhattraining/httpd-parent image..................  SUCCESS
 · Removing quay.io/mossiddi/do180-custom-httpd image..........  SUCCESS
 · Removing localhost/do180-custom-httpd image.................  SUCCESS
[student@workstation DO180-apps]$ 

