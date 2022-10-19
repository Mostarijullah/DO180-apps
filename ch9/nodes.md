  oc get nodes

  oc adm node-logs master0

  oc adm node-logs -u crio master01

  oc adm node-logs -u kubelet master01


[student@workstation DO180-apps]$ oc debug node/master01
Starting pod/master01-debug ...
To use host binaries, run `chroot /host`
Pod IP: 192.168.50.10
If you don't see a command prompt, try pressing enter.
sh-4.4# 
sh-4.4# chroot /host
sh-4.4# ls
bin   dev  home  lib64	mnt  ostree  root  sbin  sys	  tmp  var
boot  etc  lib	 media	opt  proc    run   srv	 sysroot  usr
sh-4.4# systemctl status kubelet
sh-4.4# systemctl status kubelet
● kubelet.service - Kubernetes Kubelet
   Loaded: loaded (/etc/systemd/system/kubelet.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/kubelet.service.d
           └─10-mco-default-madv.conf, 20-logging.conf, 20-nodenet.conf
   Active: active (running) since Wed 2022-10-19 02:06:10 UTC; 57min ago
  Process: 1849 ExecStartPre=/bin/rm -f /var/lib/kubelet/memory_manager_state (code=exited, status=0/SUCCESS)
  Process: 1847 ExecStartPre=/bin/rm -f /var/lib/kubelet/cpu_manager_state (code=exited, status=0/SUCCESS)
  Process: 1845 ExecStartPre=/bin/mkdir --parents /etc/kubernetes/manifests (code=exited, status=0/SUCCESS)
 Main PID: 1851 (kubelet)
    Tasks: 22 (limit: 101964)
   Memory: 264.6M
      CPU: 4min 4.077s
   CGroup: /system.slice/kubelet.service
           └─1851 kubelet --config=/etc/kubernetes/kubelet.conf --bootstrap-kubeconfig=/etc/kubernetes/kubeconf>

Oct 19 03:01:10 master01 hyperkube[1851]: I1019 03:01:10.912500    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:01:10 master01 hyperkube[1851]: I1019 03:01:10.912507    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:02:10 master01 hyperkube[1851]: I1019 03:02:10.917018    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:02:10 master01 hyperkube[1851]: I1019 03:02:10.917994    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:02:10 master01 hyperkube[1851]: I1019 03:02:10.918071    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:02:10 master01 hyperkube[1851]: I1019 03:02:10.918136    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:03:10 master01 hyperkube[1851]: I1019 03:03:10.918627    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:03:10 master01 hyperkube[1851]: I1019 03:03:10.918679    1851 kubelet_getters.go:176] "Pod status upd>
Oct 19 03:03:10 master01 hyperkube[1851]: I1019 03:03:10.918688    1851 kubelet_gette



sh-4.4# systemctl status crio   
● crio.service - Container Runtime Interface for OCI (CRI-O)
   Loaded: loaded (/usr/lib/systemd/system/crio.service; disabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/crio.service.d
           └─10-mco-default-madv.conf, 10-mco-profile-unix-socket.conf, 20-nodenet.conf
   Active: active (running) since Wed 2022-10-19 02:06:04 UTC; 58min ago
     Docs: https://github.com/cri-o/cri-o
 Main PID: 1818 (crio)
    Tasks: 37
   Memory: 6.8G
      CPU: 4min 41.701s
   CGroup: /system.slice/crio.service
           └─1818 /usr/bin/crio

Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.251637271Z" level=info msg="Pulled image: quay.i>
Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.253131530Z" level=info msg="Checking image statu>
Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.254318220Z" level=info msg="Image status: &Image>
Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.255236923Z" level=info msg="Creating container: >
Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.255357205Z" level=warning msg="Allowed annotatio>
Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.393681384Z" level=info msg="Created container 52>
Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.394271758Z" level=info msg="Starting container: >
Oct 19 02:57:26 master01 crio[1818]: time="2022-10-19 02:57:26.406261181Z" level=info msg="Started container" P>
Oct 19 03:01:10 master01 crio[1818]: time="2022-10-19 03:01:10.921198298Z" level=info msg="C


sh-4.4# cri
crictl       crio         crio-status  criu         
sh-4.4# cri
crictl       crio         crio-status  criu         
sh-4.4# crictl ps 
CONTAINER           IMAGE                                                                                                                                                   CREATED             STATE               NAME                                          ATTEMPT             POD ID
522a5f77bb3a7       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:34c5a65a07eaa06bcd2c6c16eb408f227656ecdfb1ebb334b188534f34bfa677                                  8 minutes ago       Running             container-00                                  0                   0d020369843aa
596b94d6d1dfa       image-registry.openshift-image-registry.svc:5000/developer-s2i/php-helloworld@sha256:747261f3da3893642189beb6b32a2496564b4615ec8b60abf3e6106c56ad7dc1   57 minutes ago      Running             php-helloworld                                1                   86c1bbcc92191
db61a11bd699c       f26479a95eabc6065b637a9d35beeb818e9c24c8843854968fac13fe20522528                                                                                        57 minutes ago      Running             kube-multus-additional-cni-plugins            4                   d7144c0996f7e
d22c43792d790       48b103cf871f9e76d852a8146120aad0c3b5e8b6d20e30f9ed49ddbee61a5440                                                                                        57 minutes ago      Running             openshift-apiserver-check-endpoints           3                   fd2ee6fb46f74
7090d189388f5       e3953ad34d8d0470a7fa06d50942d359f7c8b5e2242883eabbdb59ca3c83a52d                                                                                        57 minutes ago      Running             openshift-apiserver                           3                   fd2ee6fb46f74
439e14bd54638       06b3acdf60fc1ac876e697e39b9c3e5fcb69f033e152bf0f49ff6a99be883d23                                                                                        57 minutes ago      Running             kube-rbac-proxy                               4                   bea1736a8d742
8ff4fabbfa60b       06b3acdf60fc1ac876e697e39b9c3e5fcb69f033e152bf0f49ff6a99be883d23                                                                                        57 minutes ago      Running             kube-rbac-proxy                               4                   e8abda1b5a9b2
29e869e66b224       06b3acdf60fc1ac876e697e39b9c3e5fcb69f033e152bf0f49ff6a99be883d23                                              




sh-4.4# 
sh-4.4# exit
exit
sh-4.4# exit
exit

Removing debug pod ...
[student@workstation DO180-apps]$ 


