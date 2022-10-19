
[student@workstation ch9]$ oc whoami
admin

[student@workstation ch9]$ oc get nodes
NAME       STATUS   ROLES           AGE   VERSION
master01   Ready    master,worker   82d   v1.23.3+e419edf
master02   Ready    master,worker   82d   v1.23.3+e419edf
master03   Ready    master,worker   82d   v1.23.3+e419edf

[student@workstation ch9]$ oc adm top nodes
NAME       CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
master01   607m         17%    6411Mi          43%       
master02   857m         24%    7362Mi          49%       
master03   1043m        29%    8771Mi          58%  

$ oc describe TYPE NAME_PREFIX

 will first check for an exact match on TYPE and NAME_PREFIX. If no such resource exists, it will
output details for every resource that has a name prefixed with NAME_PREFIX.

 Use "oc api-resources" for a complete list of supported resources.

Usage:
  oc describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME) [flags]

Examples:
  # Describe a node
  oc describe nodes kubernetes-node-emt8.c.myproject.internal
  
  # Describe a pod
  oc describe pods/nginx
  
  # Describe a pod identified by type and name in "pod.json"
  oc describe -f pod.json
  
  # Describe all pods
  oc describe pods
  
  # Describe pods by label name=myLabel
  oc describe po -l name=myLabel
  
  # Describe all pods managed by the 'frontend' replication controller
  # (rc-created pods get the name of the rc as a prefix in the pod name)
  oc describe pods frontend

[student@workstation ch9]$ oc describe nodes
Name:               master01
Roles:              master,worker
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=master01
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
                    node-role.kubernetes.io/worker=
                    node.openshift.io/os_id=rhcos
Annotations:        machineconfiguration.openshift.io/controlPlaneTopology: HighlyAvailable
                    machineconfiguration.openshift.io/currentConfig: rendered-master-dac1e919b294c370b32172cffe4ba8e9
                    machineconfiguration.openshift.io/desiredConfig: rendered-master-dac1e919b294c370b32172cffe4ba8e9
                    machineconfiguration.openshift.io/reason: 
                    machineconfiguration.openshift.io/state: Done
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Thu, 28 Jul 2022 12:11:26 -0400
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  master01
  AcquireTime:     <unset>
  RenewTime:       Tue, 18 Oct 2022 22:32:51 -0400
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Tue, 18 Oct 2022 22:29:19 -0400   Thu, 28 Jul 2022 12:45:30 -0400   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Tue, 18 Oct 2022 22:29:19 -0400   Thu, 28 Jul 2022 12:45:30 -0400   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Tue, 18 Oct 2022 22:29:19 -0400   Thu, 28 Jul 2022 12:45:30 -0400   KubeletHasSuffici
entPID      kubelet has sufficient PID available
  Ready            True    Tue, 18 Oct 2022 22:29:19 -0400   Thu, 28 Jul 2022 12:45:41 -0400   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.50.10
  Hostname:    master01
Capacity:
  cpu:                4
  ephemeral-storage:  41407468Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             16409340Ki
  pods:               250
Allocatable:
  cpu:                3500m
  ephemeral-storage:  38161122446
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             15258364Ki
  pods:               250
System Info:
  Machine ID:                             5a24320e1fb84806a18b8cbc6b6ffc87
  System UUID:                            c4fb8d4e-a383-41ce-b33c-6ce79f1ec8de
  Boot ID:                                7f75372f-5afb-4ec6-bea2-d57cad278952
  Kernel Version:                         4.18.0-305.34.2.el8_4.x86_64
  OS Image:                               Red Hat Enterprise Linux CoreOS 410.84.202202251620-0 (Ootpa)
  Operating System:                       linux
  Architecture:                           amd64
  Container Runtime Version:              cri-o://1.23.1-9.rhaos4.10.gitbdffb9a.el8
  Kubelet Version:                        v1.23.3+e419edf
  Kube-Proxy Version:                     v1.23.3+e419edf
Non-terminated Pods:                      (32 in total)
  Namespace                               Name                                       CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                               ----                                       ------------  ----------  ---------------  -------------  ---
  common                                  nexus-1-b8w47                              0 (0%)        0 (0%)      512Mi (3%)       2Gi (13%)      82d
  developer-application                   todoapi                                    500m (14%)    500m (14%)  0 (0%)           0 (0%)         16h
  developer-s2i                           php-helloworld-6cfbd4d99b-gxsq4            0 (0%)        0 (0%)      0 (0%)           0 (0%)         15h
  openshift-apiserver                     apiserver-678b596447-t64mg                 110m (3%)     0 (0%)      250Mi (1%)       0 (0%)         82d
  openshift-authentication                oauth-openshift-656cbddb57-dzcwp           10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         82d
  openshift-cluster-node-tuning-operator  tuned-xxj7j                                10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         82d
  openshift-controller-manager            controller-manager-2bkqp                   100m (2%)     0 (0%)      100Mi (0%)       0 (0%)         17h
  openshift-dns                           dns-default-7h288                          60m (1%)      0 (0%)      110Mi (0%)       0 (0%)         82d
  openshift-dns                           node-resolver-9756h                        5m (0%)       0 (0%)      21Mi (0%)        0 (0%)         82d
  openshift-etcd                          etcd-master01                              400m (11%)    0 (0%)      930Mi (6%)       0 (0%)         82d
  openshift-etcd                          etcd-quorum-guard-7d8dc4bd99-pql5k         10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         82d
  openshift-image-registry                node-ca-94wbj                              10m (0%)      0 (0%)      10Mi (0%)        0 (0%)         82d
  openshift-ingress-canary                ingress-canary-wt7bx                       10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         82d
  openshift-kube-apiserver                kube-apiserver-guard-master01              10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         82d
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests      Limits
  --------           --------      ------
  cpu                2080m (59%)   500m (14%)
  memory             5004Mi (33%)  2Gi (13%)
  ephemeral-storage  0 (0%)        0 (0%)
  hugepages-1Gi      0 (0%)        0 (0%)
  hugepages-2Mi      0 (0%)        0 (0%)
Events:
  Type     Reason                   Age                From     Message
  ----     ------                   ----               ----     -------
  Normal   Starting                 82d                kubelet  Starting kubelet.
  Normal   NodeHasSufficientMemory  82d (x2 over 82d)  kubelet  Node master01 status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    82d (x2 over 82d)  kubelet  Node master01 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     82d (x2 over 82d)  kubelet  Node master01 status is now: NodeHasSufficientPID
  Normal   NodeAllocatableEnforced  82d                kubelet  Updated Node Allocatable limit across pods
  Normal   NodeReady                82d                kubelet  Node master01 status is now: NodeReady
  Normal   NodeNotSchedulable       82d                kubelet  Node master01 status is now: NodeNotSchedulable
  Normal   Starting                 82d                kubelet  Starting kubelet.
  Normal   NodeHasSufficientMemory  82d (x2 over 82d)  kubelet  Node master01 status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    82d (x2 over 82d)  kubelet  Node master01 status is now: NodeHasNoDiskPressure


