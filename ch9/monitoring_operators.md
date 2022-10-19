[student@workstation ch9]$ oc get clusteroperators
NAME                                       VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
authentication                             4.10.3    True        False         False      82d     
baremetal                                  4.10.3    True        False         False      82d     
cloud-controller-manager                   4.10.3    True        False         False      82d     
cloud-credential                           4.10.3    True        False         False      82d     
cluster-autoscaler                         4.10.3    True        False         False      82d     
config-operator                            4.10.3    True        False         False      82d     
console                                    4.10.3    True        False         False      28m     
csi-snapshot-controller                    4.10.3    True        False         False      82d     
dns                                        4.10.3    True        False         False      82d     
etcd                                       4.10.3    True        False         False      82d     
image-registry                             4.10.3    True        False         False      82d     
ingress                                    4.10.3    True        False         False      28m     
insights                                   4.10.3    True        False         False      82d     
kube-apiserver                             4.10.3    True        False         False      82d     
kube-controller-manager                    4.10.3    True        False         False      82d     
kube-scheduler                             4.10.3    True        False         False      82d     
kube-storage-version-migrator              4.10.3    True        False         False      17h     
machine-api                                4.10.3    True        False         False      82d     
machine-approver                           4.10.3    True        False         False      82d     
machine-config                             4.10.3    True        False         False      82d     
marketplace                                4.10.3    True        False         False      82d     
monitoring                                 4.10.3    True        False         False      82d     
network                                    4.10.3    True        False         False      82d     
node-tuning                                4.10.3    True        False         False      82d     
openshift-apiserver                        4.10.3    True        False         False      82d     
openshift-controller-manager               4.10.3    True        False         False      82d     
openshift-samples                          4.10.3    True        False         False      82d     
operator-lifecycle-manager                 4.10.3    True        False         False      82d     
operator-lifecycle-manager-catalog         4.10.3    True        False         False      82d     
operator-lifecycle-manager-packageserver   4.10.3    True        False         False      17h     
service-ca                                 4.10.3    True        False         False      82d     
storage                                    4.10.3    True        False         False      82d     
[student@workstation ch9]$ 

[student@workstation ch9]$ oc get co
NAME                                       VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
authentication                             4.10.3    True        False         False      82d     
baremetal                                  4.10.3    True        False         False      82d     
cloud-controller-manager                   4.10.3    True        False         False      82d     
cloud-credential                           4.10.3    True        False         False      82d     
cluster-autoscaler                         4.10.3    True        False         False      82d     
config-operator                            4.10.3    True        False         False      82d     
console                                    4.10.3    True        False         False      29m     


[student@workstation ch9]$ oc get ns| grep monitor
openshift-monitoring                               Active   82d
openshift-user-workload-monitoring                 Active   82d
[student@workstation ch9]$ oc get all -n openshift-monitoring 
NAME                                              READY   STATUS    RESTARTS   AGE
pod/alertmanager-main-0                           6/6     Running   18         82d
pod/alertmanager-main-1                           6/6     Running   18         82d
pod/cluster-monitoring-operator-c8969f575-4mqd9   2/2     Running   6          82d
pod/grafana-845d4d6d54-jt9q8                      3/3     Running   9          82d
pod/kube-state-metrics-896d55c47-tfstz            3/3     Running   9          82d
pod/node-exporter-7rrw9                           2/2     Running   8          82d
pod/node-exporter-jg7xm                           2/2     Running   8          82d
pod/node-exporter-tfj4j                           2/2     Running   8          82d
pod/openshift-state-metrics-6cd866ccc9-mlt29      3/3     Running   9          82d
pod/prometheus-adapter-7cbccf7496-btt6l           1/1     Running   1          17h
pod/prometheus-adapter-7cbccf7496-cd75g           1/1     Running   1          17h
pod/prometheus-k8s-0                              6/6     Running   18         82d
pod/prometheus-k8s-1                              6/6     Running   18         82d
pod/prometheus-operator-5874b67fd8-wq785          2/2     Running   6          82d
pod/thanos-querier-6fc64444c4-tp6dz               6/6     Running   18         82d
pod/thanos-querier-6fc64444c4-vfr9b               6/6     Running   18         82d

NAME                                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                               AGE
service/alertmanager-main               ClusterIP   172.30.93.6      <none>        9094/TCP,9092/TCP,9097/TCP            82d
service/alertmanager-operated           ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP            82d
service/cluster-monitoring-operator     ClusterIP   None             <none>        8443/TCP                              82d
service/grafana                         ClusterIP   172.30.217.56    <none>        3000/TCP,3002/TCP                     82d
service/kube-state-metrics              ClusterIP   None             <none>        8443/TCP,9443/TCP                     82d
service/node-exporter                   ClusterIP   None             <none>        9100/TCP                              82d
service/openshift-state-metrics         ClusterIP   None             <none>        8443/TCP,9443/TCP                     82d
service/prometheus-adapter              ClusterIP   172.30.24.236    <none>        443/TCP                               82d
service/prometheus-k8s                  ClusterIP   172.30.199.182   <none>        9091/TCP,9092/TCP                     82d
service/prometheus-k8s-thanos-sidecar   ClusterIP   None             <none>        10902/TCP                             82d
service/prometheus-operated             ClusterIP   None             <none>        9090/TCP,10901/TCP                    82d
service/prometheus-operator             ClusterIP   None             <none>        8443/TCP,8080/TCP                     82d
service/thanos-querier                  ClusterIP   172.30.244.11    <none>        9091/TCP,9092/TCP,9093/TCP,9094/TCP   82d

NAME                           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
daemonset.apps/node-exporter   3         3         3       3            3           kubernetes.io/os=linux   82d

NAME                                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cluster-monitoring-operator   1/1     1            1           82d
deployment.apps/grafana                       1/1     1            1           82d
deployment.apps/kube-state-metrics            1/1     1            1           82d
deployment.apps/openshift-state-metrics       1/1     1            1           82d
deployment.apps/prometheus-adapter            2/2     2            2           82d
deployment.apps/prometheus-operator           1/1     1            1           82d
deployment.apps/thanos-querier                2/2     2            2           82d

NAME                                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/cluster-monitoring-operator-c8969f575   1         1         1       82d
replicaset.apps/grafana-6b56dfc486                      0         0         0       82d
replicaset.apps/grafana-845d4d6d54                      1         1         1       82d
replicaset.apps/kube-state-metrics-896d55c47            1         1         1       82d
replicaset.apps/openshift-state-metrics-6cd866ccc9      1         1         1       82d
replicaset.apps/prometheus-adapter-554b645f8            0         0         0       82d
replicaset.apps/prometheus-adapter-584b695b75           0         0         0       82d
replicaset.apps/prometheus-adapter-7cbccf7496           2         2         2       17h
replicaset.apps/prometheus-operator-5874b67fd8          1         1         1       82d
replicaset.apps/thanos-querier-56d5b7b858               0         0         0       82d
replicaset.apps/thanos-querier-6fc64444c4               2         2         2       82d

NAME                                 READY   AGE
statefulset.apps/alertmanager-main   2/2     82d
statefulset.apps/prometheus-k8s      2/2     82d

NAME                                         HOST/PORT                                                      PATH   SERVICES            PORT    TERMINATION          WILDCARD
route.route.openshift.io/alertmanager-main   alertmanager-main-openshift-monitoring.apps.ocp4.example.com   /api   alertmanager-main   web     reencrypt/Redirect   None
route.route.openshift.io/grafana             grafana-openshift-monitoring.apps.ocp4.example.com                    grafana             https   reencrypt/Redirect   None
route.route.openshift.io/prometheus-k8s      prometheus-k8s-openshift-monitoring.apps.ocp4.example.com             prometheus-k8s      web     reencrypt/Redirect   None
route.route.openshift.io/thanos-querier      thanos-querier-openshift-monitoring.apps.ocp4.example.com      /api   thanos-querier      web     reencrypt/Redirect   None
[student@workstation ch9]$ 

