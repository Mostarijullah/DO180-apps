[student@workstation ~]$ lab auth-provider start

Checking prerequisites for Guided Exercise: Configuring Identity Providers

 Verify the OpenShift cluster is running:
 · Router pods are available...................................  SUCCESS
 · OAuth pods are available....................................  SUCCESS
 · API pods are available......................................  SUCCESS
 · Control plane node 'master01' is ready......................  SUCCESS
 · Control plane node 'master02' is ready......................  SUCCESS
 · Control plane node 'master03' is ready......................  SUCCESS
 Checking for conflicts with existing OpenShift projects:
 · The 'auth-provider' project is absent.......................  SUCCESS

Setting up the classroom for Guided Exercise: Configuring Identity Providers

 Preparing the student's workstation:
 · Download exercise files.....................................  SUCCESS
 · Download solution files.....................................  SUCCESS
 Restoring authentication settings to installation defaults:
 · No need to perform any change...............................  SUCCESS

Overall start status...........................................  SUCCESS

[student@workstation ~]$ source /usr/local/etc/ocp4.config
[student@workstation ~]$ htpasswd -c -B -b ~/DO280/labs/auth-provider/htpasswd \
> admin redhat
Adding password for user admin
[student@workstation ~]$ htpasswd -b ~/DO280/labs/auth-provider/htpasswd \
> developer developer
Adding password for user developer
[student@workstation ~]$ htpasswd -b ~/DO280/labs/auth-provider/htpasswd sidd redhat
Adding password for user sidd
[student@workstation ~]$ cat ~/DO280/labs/auth-provider/htpasswd


[student@workstation ~]$ oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} \
> https://api.ocp4.example.com:6443
Login successful.

You have access to 70 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
[student@workstation ~]$ oc create secret generic localusers \
> --from-file htpasswd=~/DO280/labs/auth-provider/htpasswd \
> -n openshift-config
secret/localusers created
[student@workstation ~]$ oc adm policy add-cluster-role-to-user \
> cluster-admin admin
Warning: User 'admin' not found
clusterrole.rbac.authorization.k8s.io/cluster-admin added: "admin"
[student@workstation ~]$ oc get oauth cluster \
> -o yaml > ~/DO280/labs/auth-provider/oauth.yaml 


spec:
  identityProviders:
  - htpasswd:
      fileData:
        name: localusers
    mappingMethod: claim
    name: myusers
    type: HTPasswd


NAME                                                                        ROLE                                                                                    AGE   USERS                                                            GROUPS                                         SERVICEACCOUNTS
self-provisioners                                                           ClusterRole/self-provisioner                                                            83d                                                                    system:authenticated:oauth                     
Name:         self-provisioners
Labels:       <none>
Annotations:  rbac.authorization.kubernetes.io/autoupdate: true
Role:
  Kind:  ClusterRole
  Name:  self-provisioner
Subjects:
  Kind   Name                        Namespace
  ----   ----                        ---------
  Group  system:authenticated:oauth  



[student@workstation DO180-apps]$ oc whoami
admin
[student@workstation DO180-apps]$ oc project
Using project "default" on server "https://api.ocp4.example.com:6443".
[student@workstation DO180-apps]$ oc get identities
NAME                  IDP NAME   IDP USER NAME   USER NAME     USER UID
myusers:admin         myusers    admin           admin         33f5d435-a1d7-4e44-ab34-e9fddacbf14a
myusers:developer     myusers    developer       developer     e2100fa7-fc1c-4380-be18-afd0a37b5bd4
myusers:leader        myusers    leader          leader        c63f5865-e3b9-479f-9c98-03d981b387b6
myusers:qa-engineer   myusers    qa-engineer     qa-engineer   42d05af7-44f8-4302-8b9b-a91e279dcd4e
myusers:sidd          myusers    sidd            sidd          3a86287e-e79f-404b-9caf-39e2c146a601
[student@workstation DO180-apps]$ oc get users
NAME          UID                                    FULL NAME   IDENTITIES
admin         33f5d435-a1d7-4e44-ab34-e9fddacbf14a               myusers:admin
developer     e2100fa7-fc1c-4380-be18-afd0a37b5bd4               myusers:developer
leader        c63f5865-e3b9-479f-9c98-03d981b387b6               myusers:leader
qa-engineer   42d05af7-44f8-4302-8b9b-a91e279dcd4e               myusers:qa-engineer
sidd          3a86287e-e79f-404b-9caf-39e2c146a601               myusers:sidd
[student@workstation DO180-apps]$ oc get groups
No resources found
[student@workstation DO180-apps]$ oc get clusterrolebinding -o wide | \
> grep -E 'NAME|self-provisioner' >> oauth.md 
[student@workstation DO180-apps]$ oc describe clusterrolebindings self-provisioners >> oauth.md 


[student@workstation DO180-apps]$ oc get clusterrolebinding -o wide | grep -E 'NAME|self-provisioner'
NAME                                                                        ROLE                                                                                    AGE   USERS                                                            GROUPS                                         SERVICEACCOUNTS
self-provisioners                                                           ClusterRole/self-provisioner                                                            83d                                                                    system:authenticated:oauth                     
[student@workstation DO180-apps]$ oc whoami
admin
[student@workstation DO180-apps]$ oc new project test-projectadmin
error: unknown command "new" for "oc"

Did you mean this?
	get
	new-app
	new-build
	new-project
	set
[student@workstation DO180-apps]$ oc new-project test-projectadmin
Now using project "test-projectadmin" on server "https://api.ocp4.example.com:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname


[student@workstation DO180-apps]$ oc project
Using project "test-projectadmin" on server "https://api.ocp4.example.com:6443".
[student@workstation DO180-apps]$ oc policy add-role-to-user admin leader
clusterrole.rbac.authorization.k8s.io/admin added: "leader"
[student@workstation DO180-apps]$ oc adm groups new dev-group
group.user.openshift.io/dev-group created
[student@workstation DO180-apps]$ oc adm groups add-users dev-group developer
group.user.openshift.io/dev-group added: "developer"
[student@workstation DO180-apps]$ oc adm groups new qa-group
group.user.openshift.io/qa-group created
[student@workstation DO180-apps]$ oc adm groups add-users qa-group qa-engineer
group.user.openshift.io/qa-group added: "qa-engineer"
[student@workstation DO180-apps]$ oc get groups
NAME        USERS
dev-group   developer
qa-group    qa-engineer
[student@workstation DO180-apps]$ oc login -u leader -p redhat
Login successful.

You have one project on this server: "test-projectadmin"

Using project "test-projectadmin".
[student@workstation DO180-apps]$ oc project
Using project "test-projectadmin" on server "https://api.ocp4.example.com:6443".
[student@workstation DO180-apps]$ oc policy add-role-to-group edit dev-group
clusterrole.rbac.authorization.k8s.io/edit added: "dev-group"
[student@workstation DO180-apps]$ oc policy add-role-to-group view qa-group
clusterrole.rbac.authorization.k8s.io/view added: "qa-group"
[student@workstation DO180-apps]$ oc get rolebindings -o wide
NAME                    ROLE                               AGE     USERS    GROUPS                                     SERVICEACCOUNTS
admin                   ClusterRole/admin                  3m24s   admin                                               
admin-0                 ClusterRole/admin                  2m59s   leader                                              
edit                    ClusterRole/edit                   50s              dev-group                                  
system:deployers        ClusterRole/system:deployer        3m24s                                                       test-projectadmin/deployer
system:image-builders   ClusterRole/system:image-builder   3m24s                                                       test-projectadmin/builder
system:image-pullers    ClusterRole/system:image-puller    3m24s            system:serviceaccounts:test-projectadmin   
view                    ClusterRole/view                   35s              qa-group                                   
[student@workstation DO180-apps]$ oc login -u developer -p developer
Login successful.

You have access to the following projects and can switch between them with 'oc project <projectname>':

    developer-application
    developer-s2i
  * test-projectadmin

Using project "test-projectadmin".
[student@workstation DO180-apps]$ oc new-app --name httpd httpd:latest
--> Found image d475fdc (4 months old) in image stream "openshift/httpd" under tag "latest" for "httpd:latest"

    Apache httpd 2.4 
    ---------------- 
    Apache httpd 2.4 available as container, is a powerful, efficient, and extensible web server. Apache supports a variety of features, many implemented as compiled modules which extend the core functionality. These can range from server-side programming language support to authentication schemes. Virtual hosting allows one Apache installation to serve many different Web sites.

    Tags: builder, httpd, httpd-24


--> Creating resources ...
    deployment.apps "httpd" created
    service "httpd" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/httpd' 
    Run 'oc status' to view your app.
[student@workstation DO180-apps]$ oc policy add-role-to-user edit qa-engineer
Error from server (Forbidden): rolebindings.rbac.authorization.k8s.io is forbidden: User "developer" cannot list resource "rolebindings" in API group "rbac.authorization.k8s.io" in the namespace "test-projectadmin"
[student@workstation DO180-apps]$

[student@workstation DO180-apps]$ oc login -u qa-engineer -p redhat
Login successful.

You have one project on this server: "test-projectadmin"

Using project "test-projectadmin".
[student@workstation DO180-apps]$ oc scale deployment httpd --replicas 3
Error from server (Forbidden): deployments.apps "httpd" is forbidden: User "qa-engineer" cannot patch resource "deployments/scale" in API group "apps" in the namespace "test-projectadmin"

[student@workstation DO180-apps]$ oc login -u admin -p redhat
Login successful.

You have access to 71 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "test-projectadmin".
[student@workstation DO180-apps]$ oc policy add-role-to-user admin sidd
clusterrole.rbac.authorization.k8s.io/admin added: "sidd"
[student@workstation DO180-apps]$ oc describe clusterrolebindings self-provisioners
Name:         self-provisioners
Labels:       <none>
Annotations:  rbac.authorization.kubernetes.io/autoupdate: true
Role:
  Kind:  ClusterRole
  Name:  self-provisioner
Subjects:
  Kind   Name                        Namespace
  ----   ----                        ---------
  Group  system:authenticated:oauth  



[student@workstation DO180-apps]$ oc adm policy remove-cluster-role-from-group \
> self-provisioner system:authenticated:oauth
Warning: Your changes may get lost whenever a master is restarted, unless you prevent reconciliation of this rolebinding using the following command: oc annotate clusterrolebinding.rbac self-provisioners 'rbac.authorization.kubernetes.io/autoupdate=false' --overwrite
clusterrole.rbac.authorization.k8s.io/self-provisioner removed: "system:authenticated:oauth"
[student@workstation DO180-apps]$ oc describe clusterrolebindings self-provisioners
Error from server (NotFound): clusterrolebindings.rbac.authorization.k8s.io "self-provisioners" not found
[student@workstation DO180-apps]$ oc get clusterrolebinding -o wide | \
> grep -E 'NAME|self-provisioner'
NAME                                                                        ROLE                                                                                    AGE   USERS                                                            GROUPS                                         SERVICEACCOUNTS
[student@workstation DO180-apps]$ oc adm policy add-cluster-role-to-group \
> --rolebinding-name self-provisioners \
> self-provisioner system:authenticated:oauth
Warning: Group 'system:authenticated:oauth' not found
clusterrole.rbac.authorization.k8s.io/self-provisioner added: "system:authenticated:oauth"
[student@workstation DO180-apps]$ oc describe clusterrolebindings self-provisioners
Name:         self-provisioners
Labels:       <none>
Annotations:  <none>
Role:
  Kind:  ClusterRole
  Name:  self-provisioner
Subjects:
  Kind   Name                        Namespace
  ----   ----                        ---------
  Group  system:authenticated:oauth  
[student@workstation DO180-apps]$ oc get clusterrolebinding -o wide | grep -E 'NAME|self-provisioner'
NAME                                                                        ROLE                                                                                    AGE   USERS                                                            GROUPS                                         SERVICEACCOUNTS
self-provisioners                                                           ClusterRole/self-provisioner                                                            14s                                                                    system:authenticated:oauth                     
[student@workstation DO180-apps]$ 



[student@workstation DO180-apps]$ oc explain oauth.spec.identityProviders
KIND:     OAuth
VERSION:  config.openshift.io/v1

RESOURCE: identityProviders <[]Object>

DESCRIPTION:
     identityProviders is an ordered list of ways for a user to identify
     themselves. When this list is empty, no identities are provisioned for
     users.

     IdentityProvider provides identities for users authenticating using
     credentials

FIELDS:
   basicAuth	<Object>
     basicAuth contains configuration options for the BasicAuth IdP

   github	<Object>
     github enables user authentication using GitHub credentials

   gitlab	<Object>
     gitlab enables user authentication using GitLab credentials

   google	<Object>
     google enables user authentication using Google credentials

   htpasswd	<Object>
     htpasswd enables user authentication using an HTPasswd file to validate
     credentials

   keystone	<Object>
     keystone enables user authentication using keystone password credentials

ldap	<Object>
     ldap enables user authentication using LDAP credentials

   mappingMethod	<string>
     mappingMethod determines how identities from this provider are mapped to
     users Defaults to "claim"

   name	<string>
     name is used to qualify the identities returned by this provider. - It MUST
     be unique and not shared by any other identity provider used - It MUST be a
     valid path segment: name cannot equal "." or ".." or contain "/" or "%" or
     ":" Ref:
     https://godoc.org/github.com/openshift/origin/pkg/user/apis/user/validation#ValidateIdentityProviderName

   openID	<Object>
     openID enables user authentication using OpenID credentials

   requestHeader	<Object>
     requestHeader enables user authentication using request header credentials

   type	<string>
     type identifies the identity provider type for this entry.

oc create secret generic localusers \
--from-file htpasswd=~/DO280/labs/auth-provider/htpasswd \
-n openshift-config

spec:
identityProviders:
- htpasswd:
    fileData:
      name: localusers
  mappingMethod: claim
  name: myusers
  type: HTPasswd

oc extract secret/localusers -n openshift-config \
--to ~/DO280/labs/auth-provider/ --confirm

oc set data secret/localusers \
--from-file htpasswd=~/DO280/labs/auth-provider/htpasswd \
-n openshift-config

[student@workstation DO180-apps]$ oc get secret -n openshift-config
NAME                                      TYPE                                  DATA   AGE
builder-dockercfg-lks6d                   kubernetes.io/dockercfg               1      83d
builder-token-bw2h6                       kubernetes.io/service-account-token   4      83d
builder-token-ndr59                       kubernetes.io/service-account-token   4      83d
classroom-tls                             kubernetes.io/tls                     2      83d
default-dockercfg-mbtml                   kubernetes.io/dockercfg               1      83d
default-token-chts2                       kubernetes.io/service-account-token   4      83d
default-token-t825t                       kubernetes.io/service-account-token   4      83d
deployer-dockercfg-285qk                  kubernetes.io/dockercfg               1      83d
deployer-token-5jdjq                      kubernetes.io/service-account-token   4      83d
deployer-token-vzzxt                      kubernetes.io/service-account-token   4      83d
etcd-client                               kubernetes.io/tls                     2      83d
etcd-metric-client                        kubernetes.io/tls                     2      83d
etcd-metric-signer                        kubernetes.io/tls                     2      83d
etcd-signer                               kubernetes.io/tls                     2      83d
initial-service-account-private-key       Opaque                                1      83d
localusers                                Opaque                                1      17h
pull-secret                               kubernetes.io/dockerconfigjson        1      83d
webhook-authentication-integrated-oauth   Opaque                                1      83d
[student@workstation DO180-apps]$ oc describe secret localusers 
Error from server (NotFound): secrets "localusers" not found
[student@workstation DO180-apps]$ oc describe secret localusers -n openshift-config
Name:         localusers
Namespace:    openshift-config
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
htpasswd:  299 bytes
[student@workstation DO180-apps]$ oc whoami
admin
[student@workstation DO180-apps]$ oc project
Using project "test-projectadmin" on server "https://api.ocp4.example.com:6443".

