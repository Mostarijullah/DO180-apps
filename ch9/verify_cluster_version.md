[student@workstation ch9]$ oc get clusterversion
NAME      VERSION   AVAILABLE   PROGRESSING   SINCE   STATUS
version   4.10.3    True        False         82d     Cluster version is 4.10.3

[student@workstation ch9]$ oc describe clusterversion | less


Name:         version
Namespace:    
Labels:       <none>
Annotations:  <none>
API Version:  config.openshift.io/v1
Kind:         ClusterVersion
Metadata:
  Creation Timestamp:  2022-07-28T16:10:52Z
  Generation:          2
  Managed Fields:
    API Version:  config.openshift.io/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:spec:
        .:
        f:channel:
        f:clusterID:
    Manager:      cluster-bootstrap
    Operation:    Update
    Time:         2022-07-28T16:10:52Z
    API Version:  config.openshift.io/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:status:
      f:versionHash:
    Manager:         cluster-version-operator
    Operation:       Update
    Subresource:     status
    Time:            2022-10-19T02:10:08Z
  Resource Version:  161877
  UID:               fe2b3641-43ba-4e03-8b6b-28a5696a5901
Spec:
  Channel:     stable-4.10
  Cluster ID:  c6accac3-99a2-4bf3-8d3e-cab50f629f5e
Status:
  Available Updates:
    Channels:
      candidate-4.10
      candidate-4.11
      eus-4.10
      fast-4.10
      fast-4.11
      stable-4.10
      stable-4.11
    Image:    quay.io/openshift-release-dev/ocp-release@sha256:766bc6b381163ecccfea00556dd336dd49067413f69cbc29337fea778295c7bb
    URL:      https://access.redhat.com/errata/RHBA-2022:6728
    Version:  4.10.35
    Channels:
      candidate-4.10
      candidate-4.11
      eus-4.10
      fast-4.10



[student@workstation ch9]$ oc version
Client Version: 4.10.3
Server Version: 4.10.3
Kubernetes Version: v1.23.3+e419edf



[student@workstation ch9]$ oc adm upgrade 
Cluster version is 4.10.3

Upstream is unset, so the cluster will use an appropriate default.
Channel: stable-4.10 (available channels: candidate-4.10, candidate-4.11, eus-4.10, fast-4.10, fast-4.11, stable-4.10, stable-4.11)

Recommended updates:

  VERSION     IMAGE
  4.10.35     quay.io/openshift-release-dev/ocp-release@sha256:766bc6b381163ecccfea00556dd336dd49067413f69cbc29337fea778295c7bb
  4.10.34     quay.io/openshift-release-dev/ocp-release@sha256:8e2e492710a3b36b1de781ac1e926451cbf48b566c8781fb56ce03ff22752c9b
  4.10.33     quay.io/openshift-release-dev/ocp-release@sha256:15832b3fc1eae4e3fafc97350e5d8a4d1be3948a6e5f5e9db8709b092d27e3da
  4.10.32     quay.io/openshift-release-dev/ocp-release@sha256:9f53e05393bcc9bc1ab9666b1e4307ea44be896342b3b64ab465e59bac0dbd34


