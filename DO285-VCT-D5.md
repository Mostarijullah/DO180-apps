<pre>[student@workstation ~]$ lab network-sdn start

Checking prerequisites for Guided Exercise: Troubleshooting OpenShift Software-Defined Networking

 Verify the OpenShift cluster is running:
 · Router pods are available...................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · OAuth pods are available....................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · API pods are available......................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master01&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master02&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master03&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 Checking for conflicts with existing OpenShift projects:
 · The &apos;network-sdn&apos; project is absent.........................  <font color="#34E2E2"><b>SUCCESS</b></font>

Setting up the classroom for Guided Exercise: Troubleshooting OpenShift Software-Defined Networking

 · Validate &apos;admin&apos; can log in with password &apos;redhat&apos;..........  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Validate &apos;leader&apos; can log in with password &apos;redhat&apos;.........  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Validate &apos;developer&apos; can log in with password &apos;developer&apos;...  <font color="#34E2E2"><b>SUCCESS</b></font>
 Preparing the student&apos;s workstation:
 · Download exercise files.....................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Download solution files.....................................  <font color="#34E2E2"><b>SUCCESS</b></font>

Overall start status...........................................  <font color="#34E2E2"><b>SUCCESS</b></font>

[student@workstation ~]$ oc login -u developer -p developer \
&gt; https://api.ocp4.example.com:6443
Login successful.

You have access to the following projects and can switch between them with &apos;oc project &lt;projectname&gt;&apos;:

    authorization-review
    authorization-scc
    developer-application
    developer-s2i
  * network-ingress
    test-projectadmin

Using project &quot;network-ingress&quot;.
[student@workstation ~]$ oc new-project network-sdn
Now using project &quot;network-sdn&quot; on server &quot;https://api.ocp4.example.com:6443&quot;.

You can add applications to this project with the &apos;new-app&apos; command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname

[student@workstation ~]$ cd ~/DO280/labs/network-sdn
[student@workstation network-sdn]$ oc create -f todo-db.yaml
deployment.apps/mysql created
service/mysql created
[student@workstation network-sdn]$ oc get pods
NAME                     READY   STATUS    RESTARTS   AGE
mysql-68b778f957-k5ss4   1/1     Running   0          21s
[student@workstation network-sdn]$ oc cp db-data.sql mysql-68b778f957-k5ss4:/tmp/
[student@workstation network-sdn]$ oc rsh mysql-68b778f957-k5ss4 
sh-4.4$ mysql -u root items &lt; /tmp/db-data.sql
sh-4.4$ mysql -u root items -e &quot;SHOW TABLES;&quot;
+-----------------+
| Tables_in_items |
+-----------------+
| Item            |
+-----------------+
sh-4.4$ exit
exit
[student@workstation network-sdn]$ oc create -f todo-frontend.yaml
deployment.apps/frontend created
service/frontend created
[student@workstation network-sdn]$ oc get pods
NAME                        READY   STATUS              RESTARTS   AGE
frontend-85d58fb74c-ptlwd   0/1     ContainerCreating   0          13s
mysql-68b778f957-k5ss4      1/1     Running             0          2m32s
[student@workstation network-sdn]$ oc expose service frontend \
&gt; --hostname todo.apps.ocp4.example.com
route.route.openshift.io/frontend exposed
[student@workstation network-sdn]$ oc get pods
NAME                        READY   STATUS    RESTARTS   AGE
frontend-85d58fb74c-ptlwd   1/1     Running   0          61s
mysql-68b778f957-k5ss4      1/1     Running   0          3m20s
[student@workstation network-sdn]$ oc get routes
NAME       HOST/PORT                    PATH   SERVICES   PORT   TERMINATION   WILDCARD
frontend   todo.apps.ocp4.example.com          frontend   8080                 None
[student@workstation network-sdn]$ oc describe svc/frontend
Name:              frontend
Namespace:         network-sdn
Labels:            app=todonodejs
                   name=frontend
Annotations:       &lt;none&gt;
Selector:          name=api
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.18.248
IPs:               172.30.18.248
Port:              &lt;unset&gt;  8080/TCP
TargetPort:        8080/TCP
Endpoints:         &lt;none&gt;
Session Affinity:  None
Events:            &lt;none&gt;
[student@workstation network-sdn]$ oc describe deployment/frontend | \
&gt; grep -A1 Labels
<font color="#EF2929"><b>Labels</b></font>:                 app=todonodejs
                        name=frontend
<font color="#06989A">--</font>
  <font color="#EF2929"><b>Labels</b></font>:  app=todonodejs
           name=frontend
[student@workstation network-sdn]$ oc edit svc/frontend
service/frontend edited
[student@workstation network-sdn]$ oc describe svc/frontend
Name:              frontend
Namespace:         network-sdn
Labels:            app=todonodejs
                   name=frontend
Annotations:       &lt;none&gt;
Selector:          name=frontend
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.18.248
IPs:               172.30.18.248
Port:              &lt;unset&gt;  8080/TCP
TargetPort:        8080/TCP
Endpoints:         10.8.0.41:8080
Session Affinity:  None
Events:            &lt;none&gt;
[student@workstation network-sdn]$ cd ~
[student@workstation ~]$ oc new-project network-ingress
Error from server (AlreadyExists): project.project.openshift.io &quot;network-ingress&quot; already exists
[student@workstation ~]$ lab network-policy start

Checking prerequisites for Guided Exercise: Configuring Network Policies

 Verify the OpenShift cluster is running:
 · Router pods are available...................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · OAuth pods are available....................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · API pods are available......................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master01&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master02&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master03&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 Checking for conflicts with existing OpenShift projects:
 · The &apos;network-policy&apos; project is absent......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · The &apos;network-test&apos; project is absent........................  <font color="#34E2E2"><b>SUCCESS</b></font>

Setting up the classroom for Guided Exercise: Configuring Network Policies

 · Validate &apos;admin&apos; can log in with password &apos;redhat&apos;..........  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Validate &apos;leader&apos; can log in with password &apos;redhat&apos;.........  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Validate &apos;developer&apos; can log in with password &apos;developer&apos;...  <font color="#34E2E2"><b>SUCCESS</b></font>
 Preparing Workstation:
 · Download exercise files.....................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Download solution files.....................................  <font color="#34E2E2"><b>SUCCESS</b></font>

Overall start status...........................................  <font color="#34E2E2"><b>SUCCESS</b></font>

[student@workstation ~]$ oc whoami
developer
[student@workstation ~]$ oc new-project network-policy
Now using project &quot;network-policy&quot; on server &quot;https://api.ocp4.example.com:6443&quot;.

You can add applications to this project with the &apos;new-app&apos; command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname

[student@workstation ~]$ oc new-app --name hello \
&gt; --image quay.io/redhattraining/hello-world-nginx:v1.0oc new-app --name test \
&gt; ^C
[student@workstation ~]$ oc new-app --name test \
&gt; --image quay.io/redhattraining/hello-world-nginx:v1.0
--&gt; Found container image 44eaa13 (3 years old) from quay.io for &quot;quay.io/redhattraining/hello-world-nginx:v1.0&quot;

    Red Hat Universal Base Image 8 
    ------------------------------ 
    The Universal Base Image is designed and engineered to be the base layer for all of your containerized applications, middleware and utilities. This base image is freely redistributable, but Red Hat only supports Red Hat technologies through subscriptions for Red Hat products. This image is maintained by Red Hat and updated regularly.

    Tags: base rhel8

    * An image stream tag will be created as &quot;test:v1.0&quot; that will track this image

--&gt; Creating resources ...
    imagestream.image.openshift.io &quot;test&quot; created
    deployment.apps &quot;test&quot; created
    service &quot;test&quot; created
--&gt; Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     &apos;oc expose service/test&apos; 
    Run &apos;oc status&apos; to view your app.
[student@workstation ~]$ oc new-app --name hello \
&gt; --image quay.io/redhattraining/hello-world-nginx:v1.0
--&gt; Found container image 44eaa13 (3 years old) from quay.io for &quot;quay.io/redhattraining/hello-world-nginx:v1.0&quot;

    Red Hat Universal Base Image 8 
    ------------------------------ 
    The Universal Base Image is designed and engineered to be the base layer for all of your containerized applications, middleware and utilities. This base image is freely redistributable, but Red Hat only supports Red Hat technologies through subscriptions for Red Hat products. This image is maintained by Red Hat and updated regularly.

    Tags: base rhel8

    * An image stream tag will be created as &quot;hello:v1.0&quot; that will track this image

--&gt; Creating resources ...
    imagestream.image.openshift.io &quot;hello&quot; created
    deployment.apps &quot;hello&quot; created
    service &quot;hello&quot; created
--&gt; Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     &apos;oc expose service/hello&apos; 
    Run &apos;oc status&apos; to view your app.
[student@workstation ~]$ oc expose service hello
route.route.openshift.io/hello exposed
[student@workstation ~]$ ~/DO280/labs/network-policy/display-project-info.sh
===================================================================
PROJECT: network-policy

POD NAME                 IP ADDRESS
hello-787445fd88-ft9nb   10.10.0.54
test-54bc94685b-5bwgv    10.9.0.22

SERVICE NAME   CLUSTER-IP
hello          172.30.137.163
test           172.30.138.66

ROUTE NAME   HOSTNAME                                     PORT
hello        hello-network-policy.apps.ocp4.example.com   8080-tcp

===================================================================
[student@workstation ~]$ oc new-project network-test
Now using project &quot;network-test&quot; on server &quot;https://api.ocp4.example.com:6443&quot;.

You can add applications to this project with the &apos;new-app&apos; command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname

[student@workstation ~]$ oc new-app --name sample-app \
&gt; --image quay.io/redhattraining/hello-world-nginx:v1.0
--&gt; Found container image 44eaa13 (3 years old) from quay.io for &quot;quay.io/redhattraining/hello-world-nginx:v1.0&quot;

    Red Hat Universal Base Image 8 
    ------------------------------ 
    The Universal Base Image is designed and engineered to be the base layer for all of your containerized applications, middleware and utilities. This base image is freely redistributable, but Red Hat only supports Red Hat technologies through subscriptions for Red Hat products. This image is maintained by Red Hat and updated regularly.

    Tags: base rhel8

    * An image stream tag will be created as &quot;sample-app:v1.0&quot; that will track this image

--&gt; Creating resources ...
    imagestream.image.openshift.io &quot;sample-app&quot; created
    deployment.apps &quot;sample-app&quot; created
    service &quot;sample-app&quot; created
--&gt; Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     &apos;oc expose service/sample-app&apos; 
    Run &apos;oc status&apos; to view your app.
[student@workstation ~]$ ~/DO280/labs/network-policy/display-project-info.sh
===================================================================
PROJECT: network-policy

POD NAME                 IP ADDRESS
hello-787445fd88-ft9nb   10.10.0.54
test-54bc94685b-5bwgv    10.9.0.22

SERVICE NAME   CLUSTER-IP
hello          172.30.137.163
test           172.30.138.66

ROUTE NAME   HOSTNAME                                     PORT
hello        hello-network-policy.apps.ocp4.example.com   8080-tcp

===================================================================
PROJECT: network-test

POD NAME
sample-app-7cf4f6ff64-2kxkw

===================================================================
[student@workstation ~]$ oc project network-policy
Now using project &quot;network-policy&quot; on server &quot;https://api.ocp4.example.com:6443&quot;.
[student@workstation ~]$ cd ~/DO280/labs/network-policy
[student@workstation network-policy]$ ls
allow-from-openshift-ingress.yaml  allow-specific.yaml  deny-all.yaml  <font color="#00D700">display-project-info.sh</font>
[student@workstation network-policy]$ vi d
deny-all.yaml            display-project-info.sh  
[student@workstation network-policy]$ vi deny-all.yaml 
[student@workstation network-policy]$ oc create -f deny-all.yaml
networkpolicy.networking.k8s.io/deny-all created
[student@workstation network-policy]$ ls
allow-from-openshift-ingress.yaml  allow-specific.yaml  deny-all.yaml  <font color="#00D700">display-project-info.sh</font>
[student@workstation network-policy]$ vi allow-from-openshift-ingress.yaml 
[student@workstation network-policy]$ vi allow-specific.yaml 
[student@workstation network-policy]$ oc create -n network-policy -f \
&gt; allow-specific.yaml
networkpolicy.networking.k8s.io/allow-specific created
[student@workstation network-policy]$ vi allow-from-openshift-ingress.yaml 
[student@workstation network-policy]$ oc create -n network-policy -f \
&gt; allow-from-openshift-ingress.yaml
networkpolicy.networking.k8s.io/allow-from-openshift-ingress created
[student@workstation network-policy]$ oc get networkpolicies -n network-policy
NAME                           POD-SELECTOR       AGE
allow-from-openshift-ingress   &lt;none&gt;             21s
allow-specific                 deployment=hello   2m22s
deny-all                       &lt;none&gt;             6m23s
[student@workstation network-policy]$ oc login -u admin -p redhat
Login successful.

You have access to 79 projects, the list has been suppressed. You can list all projects with &apos;oc projects&apos;

Using project &quot;network-policy&quot;.
[student@workstation network-policy]$ oc label namespace network-test \
&gt; name=network-test
namespace/network-test labeled
[student@workstation network-policy]$ oc describe namespace network-test
Name:         network-test
Labels:       kubernetes.io/metadata.name=network-test
              name=network-test
Annotations:  openshift.io/description: 
              openshift.io/display-name: 
              openshift.io/requester: developer
              openshift.io/sa.scc.mcs: s0:c28,c17
              openshift.io/sa.scc.supplemental-groups: 1000790000/10000
              openshift.io/sa.scc.uid-range: 1000790000/10000
Status:       Active

No resource quota.

No LimitRange resource.
[student@workstation network-policy]$ oc label namespace default \
&gt; network.openshift.io/policy-group=ingress
namespace/default labeled
[student@workstation network-policy]$ oc describe namespace default
Name:         default
Labels:       kubernetes.io/metadata.name=default
              network.openshift.io/policy-group=ingress
Annotations:  openshift.io/sa.scc.mcs: s0:c6,c5
              openshift.io/sa.scc.supplemental-groups: 1000040000/10000
              openshift.io/sa.scc.uid-range: 1000040000/10000
Status:       Active

No resource quota.

No LimitRange resource.
[student@workstation network-policy]$ ls
allow-from-openshift-ingress.yaml  allow-specific.yaml  deny-all.yaml  <font color="#00D700">display-project-info.sh</font>
[student@workstation network-policy]$ ./display-project-info.sh 
===================================================================
PROJECT: network-policy

POD NAME                 IP ADDRESS
hello-787445fd88-ft9nb   10.10.0.54
test-54bc94685b-5bwgv    10.9.0.22

SERVICE NAME   CLUSTER-IP
hello          172.30.137.163
test           172.30.138.66

ROUTE NAME   HOSTNAME                                     PORT
hello        hello-network-policy.apps.ocp4.example.com   8080-tcp

===================================================================
PROJECT: network-test

POD NAME
sample-app-7cf4f6ff64-2kxkw

===================================================================
[student@workstation network-policy]$ oc get networkpolicies -n network-policy
NAME                           POD-SELECTOR       AGE
allow-from-openshift-ingress   &lt;none&gt;             8m32s
allow-specific                 deployment=hello   10m
deny-all                       &lt;none&gt;             14m
[student@workstation network-policy]$ cd ~
[student@workstation ~]$ lab schedule-pods start

Checking prerequisites for Guided Exercise: Controlling Pod Scheduling Behavior

 Verify the OpenShift cluster is running:
 · Router pods are available...................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · OAuth pods are available....................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · API pods are available......................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master01&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master02&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Control plane node &apos;master03&apos; is ready......................  <font color="#34E2E2"><b>SUCCESS</b></font>

 Checking for conflicts with existing OpenShift projects:
 · The &apos;schedule-pods&apos; project is absent.......................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · The &apos;schedule-pods-ts&apos; project is absent....................  <font color="#34E2E2"><b>SUCCESS</b></font>

Setting up the classroom for Guided Exercise: Controlling Pod Scheduling Behavior

 · Validate &apos;admin&apos; can log in with password &apos;redhat&apos;..........  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Validate &apos;leader&apos; can log in with password &apos;redhat&apos;.........  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Validate &apos;developer&apos; can log in with password &apos;developer&apos;...  <font color="#34E2E2"><b>SUCCESS</b></font>

 Preparing the student&apos;s cluster:
 · Download exercise files.....................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Download solution files.....................................  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Label the first worker node with &apos;client=ACME&apos;..............  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Create project &apos;schedule-pods-ts&apos; for troubleshooting.......  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Assign &apos;edit&apos; role to &apos;developer&apos; on &apos;schedule-pods-ts&apos;.....  <font color="#34E2E2"><b>SUCCESS</b></font>
 · Deploy &apos;hello-ts&apos; application to &apos;schedule-pods-ts&apos;.........  <font color="#34E2E2"><b>SUCCESS</b></font>

Overall start status...........................................  <font color="#34E2E2"><b>SUCCESS</b></font>

[student@workstation ~]$ oc whoami
admin
[student@workstation ~]$ oc login -u developer -p developer \
&gt; https://api.ocp4.example.com:6443
Login successful.

You have access to the following projects and can switch between them with &apos;oc project &lt;projectname&gt;&apos;:

    authorization-review
    authorization-scc
    developer-application
    developer-s2i
    network-ingress
  * network-policy
    network-sdn
    network-test
    schedule-pods-ts
    test-projectadmin

Using project &quot;network-policy&quot;.
[student@workstation ~]$ oc whoami
developer
[student@workstation ~]$ oc new-project schedule-pods
Now using project &quot;schedule-pods&quot; on server &quot;https://api.ocp4.example.com:6443&quot;.

You can add applications to this project with the &apos;new-app&apos; command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname

[student@workstation ~]$ oc new-app --name hello \
&gt; --image quay.io/redhattraining/hello-world-nginx:v1.0
--&gt; Found container image 44eaa13 (3 years old) from quay.io for &quot;quay.io/redhattraining/hello-world-nginx:v1.0&quot;

    Red Hat Universal Base Image 8 
    ------------------------------ 
    The Universal Base Image is designed and engineered to be the base layer for all of your containerized applications, middleware and utilities. This base image is freely redistributable, but Red Hat only supports Red Hat technologies through subscriptions for Red Hat products. This image is maintained by Red Hat and updated regularly.

    Tags: base rhel8

    * An image stream tag will be created as &quot;hello:v1.0&quot; that will track this image

--&gt; Creating resources ...
    imagestream.image.openshift.io &quot;hello&quot; created
    deployment.apps &quot;hello&quot; created
    service &quot;hello&quot; created
--&gt; Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     &apos;oc expose service/hello&apos; 
    Run &apos;oc status&apos; to view your app.
[student@workstation ~]$ oc expose svc/hello
route.route.openshift.io/hello exposed
[student@workstation ~]$ oc get pods -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP          NODE       NOMINATED NODE   READINESS GATES
hello-787445fd88-z47f5   1/1     Running   0          22s   10.9.0.24   master01   &lt;none&gt;           &lt;none&gt;
[student@workstation ~]$ oc scale --replicas 4 deployment/hello
deployment.apps/hello scaled
[student@workstation ~]$ oc get pods -o wide
NAME                     READY   STATUS              RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
hello-787445fd88-42gt4   1/1     Running             0          6s    10.9.0.25    master01   &lt;none&gt;           &lt;none&gt;
hello-787445fd88-7zfp5   0/1     ContainerCreating   0          6s    &lt;none&gt;       master02   &lt;none&gt;           &lt;none&gt;
hello-787445fd88-gg52d   1/1     Running             0          6s    10.10.0.58   master03   &lt;none&gt;           &lt;none&gt;
hello-787445fd88-z47f5   1/1     Running             0          64s   10.9.0.24    master01   &lt;none&gt;           &lt;none&gt;
[student@workstation ~]$ oc login -u sidd -p redhat
Login successful.

You have access to 81 projects, the list has been suppressed. You can list all projects with &apos;oc projects&apos;

Using project &quot;schedule-pods&quot;.
[student@workstation ~]$ oc get nodes -L env
NAME       STATUS   ROLES           AGE   VERSION           ENV
master01   Ready    master,worker   84d   v1.23.3+e419edf   
master02   Ready    master,worker   84d   v1.23.3+e419edf   
master03   Ready    master,worker   84d   v1.23.3+e419edf   
[student@workstation ~]$ oc label node master01 city=kol-dlf1
node/master01 labeled
[student@workstation ~]$ oc label node master02 city=kol-dlf2
node/master02 labeled
[student@workstation ~]$ oc get nodes -L city
NAME       STATUS   ROLES           AGE   VERSION           CITY
master01   Ready    master,worker   84d   v1.23.3+e419edf   kol-dlf1
master02   Ready    master,worker   84d   v1.23.3+e419edf   kol-dlf2
master03   Ready    master,worker   84d   v1.23.3+e419edf   
[student@workstation ~]$ oc login -u developer -p developer
Login successful.

You have access to the following projects and can switch between them with &apos;oc project &lt;projectname&gt;&apos;:

    authorization-review
    authorization-scc
    developer-application
    developer-s2i
    network-ingress
    network-policy
    network-sdn
    network-test
  * schedule-pods
    schedule-pods-ts
    test-projectadmin

Using project &quot;schedule-pods&quot;.
[student@workstation ~]$ oc edit deployment/hello
deployment.apps/hello edited
[student@workstation ~]$ oc get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE   IP          NODE       NOMINATED NODE   READINESS GATES
hello-6d665ff98-bpq7k   1/1     Running   0          18s   10.9.0.26   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-l2cj9   1/1     Running   0          14s   10.9.0.29   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-q8xrd   1/1     Running   0          15s   10.9.0.28   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-xbnkr   1/1     Running   0          18s   10.9.0.27   master01   &lt;none&gt;           &lt;none&gt;
[student@workstation ~]$ oc login -u admin -p redhat
Login successful.

You have access to 81 projects, the list has been suppressed. You can list all projects with &apos;oc projects&apos;

Using project &quot;schedule-pods&quot;.
[student@workstation ~]$ oc label node -l city city-
node/master01 unlabeled
node/master02 unlabeled
[student@workstation ~]$ oc login -u developer -p developer
Login successful.

You have access to the following projects and can switch between them with &apos;oc project &lt;projectname&gt;&apos;:

    authorization-review
    authorization-scc
    developer-application
    developer-s2i
    network-ingress
    network-policy
    network-sdn
    network-test
  * schedule-pods
    schedule-pods-ts
    test-projectadmin

Using project &quot;schedule-pods&quot;.
[student@workstation ~]$ oc get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP          NODE       NOMINATED NODE   READINESS GATES
hello-6d665ff98-bpq7k   1/1     Running   0          2m12s   10.9.0.26   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-l2cj9   1/1     Running   0          2m8s    10.9.0.29   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-q8xrd   1/1     Running   0          2m9s    10.9.0.28   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-xbnkr   1/1     Running   0          2m12s   10.9.0.27   master01   &lt;none&gt;           &lt;none&gt;
[student@workstation ~]$ oc delete pod hello-6d665ff98-xbnkr
pod &quot;hello-6d665ff98-xbnkr&quot; deleted
[student@workstation ~]$ oc delete pod hello-6d665ff98-q8xrd 
pod &quot;hello-6d665ff98-q8xrd&quot; deleted
[student@workstation ~]$ oc get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP          NODE       NOMINATED NODE   READINESS GATES
hello-6d665ff98-bpq7k   1/1     Running   0          2m51s   10.9.0.26   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-l2cj9   1/1     Running   0          2m47s   10.9.0.29   master01   &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-mrpzs   0/1     Pending   0          17s     &lt;none&gt;      &lt;none&gt;     &lt;none&gt;           &lt;none&gt;
hello-6d665ff98-t7jfd   0/1     Pending   0          5s      &lt;none&gt;      &lt;none&gt;     &lt;none&gt;           &lt;none&gt;
[student@workstation ~]$ oc describe pod hello-6d665ff98-t7jfd 
Name:           hello-6d665ff98-t7jfd
Namespace:      schedule-pods
Priority:       0
Node:           &lt;none&gt;
Labels:         deployment=hello
                pod-template-hash=6d665ff98
Annotations:    openshift.io/generated-by: OpenShiftNewApp
                openshift.io/scc: restricted
Status:         Pending
IP:             
IPs:            &lt;none&gt;
Controlled By:  ReplicaSet/hello-6d665ff98
Containers:
  hello:
    Image:        quay.io/redhattraining/hello-world-nginx@sha256:941928d702a2f08c986017b1eed3417d83952f05de55d657787512e82714dd89
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  &lt;none&gt;
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-slt9x (ro)
Conditions:
  Type           Status
  PodScheduled   False 
Volumes:
  kube-api-access-slt9x:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       &lt;nil&gt;
    DownwardAPI:             true
    ConfigMapName:           openshift-service-ca.crt
    ConfigMapOptional:       &lt;nil&gt;
QoS Class:                   BestEffort
Node-Selectors:              city=kol-dlf1
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  34s   default-scheduler  0/3 nodes are available: 3 node(s) didn&apos;t match Pod&apos;s node affinity/selector.
[student@workstation ~]$ oc edit deployment/hello-ts
Error from server (NotFound): deployments.apps &quot;hello-ts&quot; not found
[student@workstation ~]$ oc edit deployment/hello
deployment.apps/hello edited
[student@workstation ~]$ oc get pods -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
hello-787445fd88-dvghj   1/1     Running   0          6s    10.9.0.31    master01   &lt;none&gt;           &lt;none&gt;
hello-787445fd88-lwrmp   1/1     Running   0          8s    10.8.0.43    master02   &lt;none&gt;           &lt;none&gt;
hello-787445fd88-phhrb   1/1     Running   0          10s   10.10.0.60   master03   &lt;none&gt;           &lt;none&gt;
hello-787445fd88-xlcd4   1/1     Running   0          10s   10.9.0.30    master01   &lt;none&gt;           &lt;none&gt;
[student@workstation ~]$ 
</pre>
