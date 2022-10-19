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



