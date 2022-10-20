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


