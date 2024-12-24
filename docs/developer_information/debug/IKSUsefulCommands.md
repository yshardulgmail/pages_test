---
layout: default
title: IKS Useful Commands
parent: Debug
grand_parent: Developer Information
---




#  RIAS / RIAS IKS Useful Commands





 
 
 
 
 
 
 
 
 Created by  Jeffrey Zarend, last modified on Feb 22, 2022
 

A lot of the IBMCLOUD Cli commands assume you can log in to the Balaji account.  All RIAS IKS clusters belong under this account.   The commands to get/download and set your kubernetes context to your cluster is documented in multiple spots

* **dev09**  \= mzone7212
* **dev29** \= mzone230j
* **dev55** \= mzone7203
* **dev56** \= mzone7204
* **dev39** \= mzone7324

  


* [https://github.ibm.com/genctl\-cicd/micro\-deploy\-server](https://github.ibm.com/genctl-cicd/micro-deploy-server)     ← MDS Documentation.  Required setup instructions before you can deploy rias
* [razee Tips](https://confluence.swg.usma.ibm.com:8445/display/CTL/razee+Tips)    ← Mike George's Razee tips
* [Developer Guide](https://confluence.swg.usma.ibm.com:8445/display/CLD/Developer+Guide) (MASCD reference page/documentation)
* ```
Find the cluster name.  This example uses dev29, which is mzone230j  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ ic ks clusters | grep dev29  
rias-ng-us-south-dal-dev29-etcd   bqq878rd05den5jpjuo0   normal        2 years ago    6         Dallas     1.20.15_1568*   Default               classic     
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$   
  

```
* ```
Find your current context:  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kubectl config current-context  
rias-ng-us-south-dal-dev29-etcd/bqq878rd05den5jpjuo0  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$  
  

```
* ```
List kubernetes pods (output truncated and shortened for brevity)  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kubectl get pods -A  
NAMESPACE             NAME                                                        READY   STATUS      RESTARTS   AGE  
default               logdna-agent-72j8z                                          1/1     Running     0          7d2h  
ibm-services-system   bes-local-client-4k95p                                      1/1     Running     0          90d  
ibm-services-system   crowdstrike-7jgdj                                           1/1     Running     1          90d  
ibm-system            addon-catalog-source-lzl5l                                  1/1     Running     0          7d2h  
kube-system           calico-kube-controllers-8697b879bf-zrbdx                    1/1     Running     0          65d  
kube-system           vpn-68fb457f55-rwpdl                                        1/1     Running     0          17d  
razee                 featureflagsetld-controller-64544bd6d6-8cmrj                1/1     Running     0          19h  
razee                 mustachetemplate-controller-ccc699cc6-wl24r                 1/1     Running     0          19h  
razee                 remoteresources3-controller-7f9f5c7d97-kz9gr                1/1     Running     0          19h  
riaascore             nginx-ingress-controller-64b9dd5559-6ll6g                   1/1     Running     2          7d2h  
riaascore             nginx-ingress-controller-64b9dd5559-j9nct                   1/1     Running     2          7d2h  
riaasiam              regional-authorization-cache-master-0-57fc9f84bb-qnfvv      1/1     Running     0          17h  
riaasstorage          regional-storage-7dbf94746d-jwxkh                           1/1     Running     0          4h51m  
rias-etcd             etcd-backup-operator-85bb7554f9-hs4g6                       1/1     Running     1          7d2h  
rias-etcd             etcd-cluster-hxzmktltnq                                     1/1     Running     0          7d2h  
rias-etcd             etcd-cluster-l6hjwt67qw                                     1/1     Running     0          7d2h  
rias-etcd             etcd-cluster-lrm5bkgk8q                                     1/1     Running     0          7d2h  
rias-etcd             etcd-operator-54577f75cf-bfgw7                              1/1     Running     0          7d2h  
rias-etcd             etcd-restore-operator-c5dc8d7b7-54zsl                       1/1     Running     0          7d2h  
rias-flink            regional-group-flink-job-7f58b77c7d-x4t6b                   1/1     Running     1          17h  
rias-flink            regional-group-flink-worker-7f65f8d657-g9fbg                1/1     Running     0          17h  
rias                  bss-integration-server-76847b78bf-2gwsk                     1/1     Running     1          2d15h  
rias                  keyreact-0                                                  1/1     Running     0          7d2h  
rias                  keyreact-grpc-0                                             1/1     Running     0          7d2h  
rias                  keyreact-grpc-file-0                                        1/1     Running     0          7d2h  
sysdig                sysdig-agent-dmf2x                                          1/1     Running     0          4d1h  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$  
  

```
* ```
Find mis-behaving or pods with errors.  Best thing to do is kill the pod  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kubectl get pods -A | egrep -v "Running|Completed"  
NAMESPACE             NAME                                                        READY   STATUS      RESTARTS   AGE  
ibm-services-system   kube-auditlog-forwarder-684c979576-2gk4c                    0/1     Evicted     0          40d  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$   
  

```
* ```
To lock an entire cluster so Razee stops updating it  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ cat lock3.sh   
if ! ( kubectl patch configmap/razeedeploy-config -n razee --type merge -p '{"data":{"lock-cluster": "true"}}' 2> /dev/null ); then  
  kubectl create configmap razeedeploy-config -n razee --from-literal=lock-cluster=true  
  kubectl delete pods -n razee $(kubectl get pods -n razee| grep mustachetemplate-controller | awk '{print $1}' | paste -s -d ',' -)   
fi  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$   
  

```
* ```
To unlock an entire cluster so Razee resumes maintaining it  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ cat unlock3.sh   
kubectl patch configmap/razeedeploy-config -n razee --type merge -p '{"data":{"lock-cluster": "false"}}'  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$   
  

```
* ```
Find out if a cluster is or is not locked to Razee (this example shows false, so its not locked)  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kubectl describe cm razeedeploy-config -n razee  
Name:         razeedeploy-config  
Namespace:    razee  
Labels:       <none>  
Annotations:  <none>  
  
Data  
====  
lock-cluster:  
----  
false  
Events:  <none>  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$   
  

```
* ```
Command to determine Razee deployed versions (abreviated for brevity)  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kubectl get ffsld -n rias rias-ffs-ld -o yaml   
apiVersion: [deploy.razee.io/v1alpha2](http://deploy.razee.io/v1alpha2)  
data:  
  acadia-storage-image-version: 1.73.0  
  aew-image-version: 1.51.0  
  agw-image-version: 1.4.0  
  ap-image-version: 3dbf0ba4e9d13b02de01afe27ecf86bb781c63f1  
  aw-image-version: 61b544fa9b50cc96d488adc59a46b50a4c7ca809  
  baremetal-image-version: 1.26.0  
  biw-image-version: 1.22.0  
  cdho-image-version: 1.10.0  
  cloudinit-image-version: 1.11.0  
  rdcw-image-version: 1.9.0  
  rdw-image-version: 1.18.0  
  region-globals: rias-ng-us-south-dal-dev29-etcd  
  tw-image-version: 1.18.0  
  vsb-image-version: 959a4a9110436c944acb043bb5f90814c1ddd2aa  
  zsw-image-version: 32e60cb76413f2fd257775c805bc4a7717be13c7  
kind: FeatureFlagSetLD  
metadata:  
  annotations:  
    [deploy.razee.io/data-hash](http://deploy.razee.io/data-hash): 71b243a23e689656bb99757d46342b9fe5435e0d  
    [kubectl.kubernetes.io/last-applied-configuration](http://kubectl.kubernetes.io/last-applied-configuration): |  
      {"apiVersion":"deploy.razee.io/v1alpha2","data":{"region-globals":"rias-ng-us-south-dal-dev29-etcd"},"kind":"FeatureFlagSetLD","metadata":{"annotations":{},"name":"rias-ffs-ld","namespace":"rias"},"spec":{"identityRef":{"env":[{"name":"region","value":"rias-ng-us-south-dal-dev29-etcd"}]},"sdkKeyRef":{"valueFrom":{"secretKeyRef":{"key":"ld-sdk-key","name":"ld-sdk-key"}}}}}  
  creationTimestamp: "2022-02-08T09:40:35Z"  
status:  
  FeatureFlagUpdateReceived: "2022-02-15T12:30:36.091Z"  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$   
  

```
* ```
List namespaces  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kubectl get namespace -A  
NAME                               STATUS   AGE  
2c74fd3b89e64077afd34d8ab8af4f09   Active   7d3h  
8c998e95615646bdae948214c4c8d8bc   Active   7d3h  
a2565b70e79640e68173531efdbbfb0f   Active   7d3h  
a84a3ddbb52c469ab308c8b5ae300b55   Active   7d3h  
aa5a471f75bc456fac416bf02c4ba6de   Active   7d3h  
c21088ea7a014ffb8f6a7bc4304699a0   Active   7d3h  
ddcc6e01de344d2783d8e1ddbe88294d   Active   7d3h  
default                            Active   648d  
fed0f96b3aea4c10954ed825d3b4b4ae   Active   7d3h  
ibm-cert-store                     Active   648d  
ibm-operators                      Active   524d  
ibm-services-system                Active   90d  
ibm-system                         Active   648d  
jaeger                             Active   7d3h  
kube-node-lease                    Active   648d  
kube-public                        Active   648d  
kube-system                        Active   648d  
razee                              Active   7d3h  
riaascore                          Active   7d3h  
riaasiam                           Active   7d3h  
riaasstorage                       Active   7d3h  
rias                               Active   7d3h  
rias-etcd                          Active   7d3h  
rias-flink                         Active   7d3h  
sysdig                             Active   7d3h  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$   
  
  
  

```
* ```
Dump Razee log (in this case for mzone7204/cluster dev56  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kgct  
rias-ng-us-south-dal-dev56-etcd/bqt1hctd0o4i5ivs4ik0  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ kubectl get -A mtp,rrs3 -o=json | jq '.items[] | select(.status."razee-logs".error) | .[metadata.name](http://metadata.name),.metadata.namespace,.kind,.status."razee-logs"'  
"inception-remote-resource"  
"rias-etcd"  
"RemoteResourceS3"  
{  
  "error": {  
    "5b506f50f47906f9fda265652e536675ecfeadb7": "uri: <https://s3.us-south.cloud-object-storage.appdomain.cloud/development-workspace-artifacts/rias-etcd-globals//dev-szr/common-globals.yaml>, statusCode: 404, message: undefined"  
  }  
}  
Jeffreys-MacBook-Pro:rias-maint jeffreyjzarend$ 
```



 


Document generated by Confluence on Jul 15, 2024 13:04


[Atlassian](https://www.atlassian.com/)


 


