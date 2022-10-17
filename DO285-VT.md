OCP4.10

Error -

[student@workstation DO180-apps]$ git push
usage: git credential-cache [<options>] <action>

    --timeout <n>         number of seconds to cache credentials
    --socket <path>       path of cache-daemon socket

git credential-cache --timeout=3600
 get: line 1: get: command not found
Gtk-Message: 02:33:12.604: Failed to load module "canberra-gtk-module"
Gtk-Message: 02:33:21.276: Failed to load module "canberra-gtk-module"
usage: git credential-cache [<options>] <action>

    --timeout <n>         number of seconds to cache credentials
    --socket <path>       path of cache-daemon socket

git credential-cache --timeout=3600
 store: line 1: store: command not found


solution -

[student@workstation DO180-apps]$ sudo yum install libcanberra-gtk2
Last metadata expiration check: 3:52:19 ago on Sun 16 Oct 2022 11:11:50 PM EDT.
Dependencies resolved.
================================================================================================================
 Package                  Architecture   Version               Repository                                  Size
================================================================================================================
Installing:
 libcanberra-gtk2         x86_64         0.30-18.el8           rhel-8.6-for-x86_64-appstream-rpms          33 k

Transaction Summary
================================================================================================================
Install  1 Package

Total download size: 33 k
Installed size: 51 k
Is this ok [y/N]: y
Downloading Packages:
libcanberra-gtk2-0.30-18.el8.x86_64.rpm                                         1.7 MB/s |  33 kB     00:00    
----------------------------------------------------------------------------------------------------------------
Total                                                                           1.5 MB/s |  33 kB     00:00     
Running transaction check

