---
layout: default
title: Update IKS cluster 
parent: Developer Information
---


# Update IKS cluster


\# Get the IKS Versions  


ibmcloud ks versions

```bash 
shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks versions

**OK**

**Kubernetes Versions**   

**1\.22\.15**   

**1\.23\.12 (default)**   

**1\.24\.6**   

  


**OpenShift Versions**   

**4\.6\.61\_openshift** **(deprecated, unsupported in 24 days)**   

**4\.7\.59\_openshift** **(deprecated, unsupported in 65 days)**   

**4\.8\.49\_openshift**   

**4\.9\.48\_openshift (default)**   

**4\.10\.32\_openshift**   

**4\.11\.4\_openshift**   
```


To assess the differences across versions, see **'[https://ibm.biz/iks\-versions](https://ibm.biz/iks-versions)'**.

  
\# Get Nodes to see current IKS Version  
kubectl get nodes

```bash
shashikantshelke@Shashikants\-MacBook\-Pro config % kubectl get nodes

NAME             STATUS   ROLES    AGE      VERSION

10\.186\.36\.35     Ready    \<none\>   2y135d   v1\.22\.12\+IKS

10\.186\.36\.58     Ready    \<none\>   2y135d   v1\.22\.12\+IKS

10\.241\.150\.156   Ready    \<none\>   2y135d   v1\.22\.12\+IKS

10\.241\.150\.162   Ready    \<none\>   2y135d   v1\.22\.12\+IKS

10\.95\.236\.119    Ready    \<none\>   2y135d   v1\.22\.12\+IKS

10\.95\.236\.79     Ready    \<none\>   2y135d   v1\.22\.12\+IKS

  
\# **Update master,** specify the k8s version and cluster name  
ibmcloud ks cluster master update \-\-version $k8s\_version \-c $cluster

```bash
shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks cluster master update \-\-version 1\.23\.12 \-c rias\-ng\-us\-south\-dal\-dev55\-etcd

During the update, you cannot access or change the cluster. Worker nodes, apps, and resources that have been deployed by the user are not modified and will continue to run.

You might need to change your YAML files for future deployments. Review the docs for details: **'[https://ibm.biz/iks\-versions](https://ibm.biz/iks-versions)'**.

Are you sure you want to update your Kubernetes API server rias\-ng\-us\-south\-dal\-dev55\-etcd to 1\.23\.12? \[y/N]**\>** y

Updating cluster **rias\-ng\-us\-south\-dal\-dev55\-etcd**...

**OK**

**To update to 1\.23\.12 version, run 'ibmcloud ks worker update \-\-cluster rias\-ng\-us\-south\-dal\-dev55\-etcd \-\-workers \<worker1 ID\>\[,\<worker2 ID\>,...]'. Review and make any required version changes before you update: '<https://ibm.biz/upworker>'**
```
  
// wait 20 mins to make sure it has pulled k8s v1\.17, to check:  
ibmcloud ks cluster get \-c $cluster

```bash
shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks cluster get \-c rias\-ng\-us\-south\-dal\-dev55\-etcd

Retrieving cluster **rias\-ng\-us\-south\-dal\-dev55\-etcd**...

**OK**

                                   

**Name:**                           rias\-ng\-us\-south\-dal\-dev55\-etcd   

**ID:**                             bqt1gpfd06fjna35h3d0   

**State:**                          normal   

**Status:**                         All Workers Normal   

**Created:**                        2020\-05\-12T03:30:45\+0000   

**Location:**                       dal10   

**Pod Subnet:**                     172\.30\.0\.0/16   

**Service Subnet:**                 172\.21\.0\.0/16   

**Master URL:**                     [https://c108\.us\-south.containers.cloud.ibm.com:30325](https://c108.us-south.containers.cloud.ibm.com:30325)   

**Public Service Endpoint URL:**    [https://c108\.us\-south.containers.cloud.ibm.com:30325](https://c108.us-south.containers.cloud.ibm.com:30325)   

**Private Service Endpoint URL:**   [https://c108\.private.us\-south.containers.cloud.ibm.com:30325](https://c108.private.us-south.containers.cloud.ibm.com:30325)   

**Master Location:**                Dallas   

**Master Status:**                  Version update requested. (39 seconds ago)   

**Master State:**                   deployed   

**Master Health:**                  normal   

**Ingress Subdomain:**              [rias\-ng\-us\-south\-626350\-e0fceba10d5eb16d7f429bdcab10fe80\-0000\.us](http://rias-ng-us-south-626350-e0fceba10d5eb16d7f429bdcab10fe80-0000.us)\-south.containers.appdomain.cloud   

**Ingress Secret:**                 rias\-ng\-us\-south\-626350\-e0fceba10d5eb16d7f429bdcab10fe80\-0000   

**Ingress Status:**                 healthy   

**Ingress Message:**                All Ingress components are healthy   

**Workers:**                        6   

**Worker Zones:**                   dal10, dal12, dal13   

**Version:**                        1\.22\.15\_1572 \-\-\> 1\.23\.12\_1546 (pending)   

**Creator:**                        \-   

**Monitoring Dashboard:**           \-   

**Resource Group ID:**              d9ab4ee69a154af69d6fc53f7ed49b89   

**Resource Group Name:**            Default   

  


shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks cluster get \-c rias\-ng\-us\-south\-dal\-dev55\-etcd

Retrieving cluster **rias\-ng\-us\-south\-dal\-dev55\-etcd**...

**OK**

                                   

**Name:**                           rias\-ng\-us\-south\-dal\-dev55\-etcd   

**ID:**                             bqt1gpfd06fjna35h3d0   

**State:**                          normal   

**Status:**                         All Workers Normal   

**Created:**                        2020\-05\-12T03:30:45\+0000   

**Location:**                       dal10   

**Pod Subnet:**                     172\.30\.0\.0/16   

**Service Subnet:**                 172\.21\.0\.0/16   

**Master URL:**                     [https://c108\.us\-south.containers.cloud.ibm.com:30325](https://c108.us-south.containers.cloud.ibm.com:30325)   

**Public Service Endpoint URL:**    [https://c108\.us\-south.containers.cloud.ibm.com:30325](https://c108.us-south.containers.cloud.ibm.com:30325)   

**Private Service Endpoint URL:**   [https://c108\.private.us\-south.containers.cloud.ibm.com:30325](https://c108.private.us-south.containers.cloud.ibm.com:30325)   

**Master Location:**                Dallas   

**Master Status:**                  Ready (2 minutes ago)   

**Master State:**                   deployed   

**Master Health:**                  normal   

**Ingress Subdomain:**              [rias\-ng\-us\-south\-626350\-e0fceba10d5eb16d7f429bdcab10fe80\-0000\.us](http://rias-ng-us-south-626350-e0fceba10d5eb16d7f429bdcab10fe80-0000.us)\-south.containers.appdomain.cloud   

**Ingress Secret:**                 rias\-ng\-us\-south\-626350\-e0fceba10d5eb16d7f429bdcab10fe80\-0000   

**Ingress Status:**                 healthy   

**Ingress Message:**                All Ingress components are healthy   

**Workers:**                        6   

**Worker Zones:**                   dal10, dal12, dal13   

**Version:**                        1\.23\.12\_1546   

**Creator:**                        \-   

**Monitoring Dashboard:**           \-   

**Resource Group ID:**              d9ab4ee69a154af69d6fc53f7ed49b89   

**Resource Group Name:**            Default 


```
**update worker nodes**  
// get list of worker nodes  
ibmcloud ks worker ls \-c $cluster

```bash
shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks worker ls \-c rias\-ng\-us\-south\-dal\-dev55\-etcd

**OK**

**ID**                                                       **Public IP**        **Private IP**       **Flavor**               **State**    **Status**   **Zone**    **Version**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738**   150\.238\.90\.98    10\.186\.36\.58     b3c.4x16\.encrypted   normal   Ready    dal13   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000880**   150\.238\.90\.107   10\.186\.36\.35     b3c.4x16\.encrypted   normal   Ready    dal13   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000947**   169\.63\.26\.100    10\.241\.150\.156   b3c.4x16\.encrypted   normal   Ready    dal12   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000a61**   169\.63\.26\.117    10\.241\.150\.162   b3c.4x16\.encrypted   normal   Ready    dal12   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000bb6**   150\.238\.25\.24    10\.95\.236\.79     b3c.4x16\.encrypted   normal   Ready    dal10   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000c5b**   150\.238\.25\.20    10\.95\.236\.119    b3c.4x16\.encrypted   normal   Ready    dal10   **1\.22\.12\_1566†**   

  


**†** **The worker Kubernetes version is deprecated. For more information, run 'ibmcloud ks worker get'**

  


**\*** **To update to 1\.23\.12\_1548 version, run 'ibmcloud ks worker update'. Review and make any required version changes before you update: '<https://ibm.biz/upworker>'**

```

// update worker node, repeat the command below for each worker node  
// Do wait 20\-30 mins to make sure the node has been upgraded, you can check the status from the above command.  
ibmcloud ks worker update \-\-cluster $cluster \-\-worker $worker\_id

```bash
shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks worker update \-\-cluster rias\-ng\-us\-south\-dal\-dev55\-etcd \-\-worker kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738

Updating the worker node version can cause downtime for your apps and services. During the update, all pods might be rescheduled onto other worker nodes and data is deleted if not stored outside the pod. To avoid downtime, ensure that you have enough worker nodes to handle your workload while the selected worker nodes are updating.

You might need to change your YAML files for deployments before updating. Review the docs for details: **'<https://ibm.biz/upworker>'**

Are you sure you want to update your worker node \[kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738] to 1\.23\.12\_1548? \[y/N]**\>** y

Processing **kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738**...

Processing on **kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738** complete.

**OK**

shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks worker ls \-c rias\-ng\-us\-south\-dal\-dev55\-etcd                                                   

**OK**

**ID**                                                       **Public IP**        **Private IP**       **Flavor**               **State**    **Status**   **Zone**    **Version**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738**   150\.238\.90\.98    10\.186\.36\.58     b3c.4x16\.encrypted   normal   Ready    dal13   1\.22\.12\_1566 \-\-\> 1\.23\.12\_1548 (pending)   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000880**   150\.238\.90\.107   10\.186\.36\.35     b3c.4x16\.encrypted   normal   Ready    dal13   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000947**   169\.63\.26\.100    10\.241\.150\.156   b3c.4x16\.encrypted   normal   Ready    dal12   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000a61**   169\.63\.26\.117    10\.241\.150\.162   b3c.4x16\.encrypted   normal   Ready    dal12   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000bb6**   150\.238\.25\.24    10\.95\.236\.79     b3c.4x16\.encrypted   normal   Ready    dal10   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000c5b**   150\.238\.25\.20    10\.95\.236\.119    b3c.4x16\.encrypted   normal   Ready    dal10   **1\.22\.12\_1566†**   

  


**†** **The worker Kubernetes version is deprecated. For more information, run 'ibmcloud ks worker get'**

  


**\*** **To update to 1\.23\.12\_1548 version, run 'ibmcloud ks worker update'. Review and make any required version changes before you update: '<https://ibm.biz/upworker>'**

  


shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks worker ls \-c rias\-ng\-us\-south\-dal\-dev55\-etcd

**OK**

**ID**                                                       **Public IP**        **Private IP**       **Flavor**               **State**    **Status**   **Zone**    **Version**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738**   150\.238\.90\.98    10\.186\.36\.58     b3c.4x16\.encrypted   normal   Ready    dal13   1\.23\.12\_1548   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000880**   150\.238\.90\.107   10\.186\.36\.35     b3c.4x16\.encrypted   normal   Ready    dal13   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000947**   169\.63\.26\.100    10\.241\.150\.156   b3c.4x16\.encrypted   normal   Ready    dal12   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000a61**   169\.63\.26\.117    10\.241\.150\.162   b3c.4x16\.encrypted   normal   Ready    dal12   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000bb6**   150\.238\.25\.24    10\.95\.236\.79     b3c.4x16\.encrypted   normal   Ready    dal10   **1\.22\.12\_1566†**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000c5b**   150\.238\.25\.20    10\.95\.236\.119    b3c.4x16\.encrypted   normal   Ready    dal10   **1\.22\.12\_1566†**   

  


**†** **The worker Kubernetes version is deprecated. For more information, run 'ibmcloud ks worker get'**

  


**\*** **To update to 1\.23\.12\_1548 version, run 'ibmcloud ks worker update'. Review and make any required version changes before you update: '<https://ibm.biz/upworker>'**

  


shashikantshelke@Shashikants\-MacBook\-Pro config % ibmcloud ks worker ls \-c rias\-ng\-us\-south\-dal\-dev55\-etcd                                                                        

**OK**

**ID**                                                       **Public IP**        **Private IP**       **Flavor**               **State**    **Status**   **Zone**    **Version**   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000738**   150\.238\.90\.98    10\.186\.36\.58     b3c.4x16\.encrypted   normal   Ready    dal13   1\.23\.12\_1548   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000880**   150\.238\.90\.107   10\.186\.36\.35     b3c.4x16\.encrypted   normal   Ready    dal13   1\.23\.12\_1548   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000947**   169\.63\.26\.100    10\.241\.150\.156   b3c.4x16\.encrypted   normal   Ready    dal12   1\.23\.12\_1548   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000a61**   169\.63\.26\.117    10\.241\.150\.162   b3c.4x16\.encrypted   normal   Ready    dal12   1\.23\.12\_1548   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000bb6**   150\.238\.25\.24    10\.95\.236\.79     b3c.4x16\.encrypted   normal   Ready    dal10   1\.23\.12\_1548   

**kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000c5b**   150\.238\.25\.20    10\.95\.236\.119    b3c.4x16\.encrypted   normal   Ready    dal10   1\.23\.12\_1548   

  


shashikantshelke@Shashikants\-MacBook\-Pro config %       ibmcloud ks worker get \-\-cluster rias\-ng\-us\-south\-dal\-dev55\-etcd \-\-worker kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000c5b

Retrieving worker **kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000c5b**...

**OK**

                   

**ID:**             kube\-bqt1gpfd06fjna35h3d0\-riasngussou\-etcdwri\-00000c5b   

**State:**          normal   

**Status:**         Ready   

**Private VLAN:**   2874248   

**Public VLAN:**    2874246   

**Private IP:**     10\.95\.236\.119   

**Public IP:**      150\.238\.25\.20   

**Hardware:**       dedicated   

**Pool Name:**      etcdwrias   

**Pool ID:**        bqt1gpfd06fjna35h3d0\-ed84e31   

**Zone:**           dal10   

**Flavor:**         b3c.4x16\.encrypted   

**Version:**        1\.23\.12\_1548
```
  
// check if pods are in running state, if not then wait a couple of mins  
kubectl get pods \-o wide \-A   

  




 


Document generated by Confluence on Jul 15, 2024 13:04


[Atlassian](https://www.atlassian.com/)


 


