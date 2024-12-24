---
layout: default
title: HostOS mzone Inventory 
parent: Hardware
---




## HostOS mzone Inventory


This page contains a table for mzones that have been assigned for use by the HostOS development team.


## References


| Item | Description | Notes |
| --- | --- | --- |
| [mzone files](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region) | This is the repo where mzonexxx yml files are located. | Fetch the latest master branch of this repo to insure you have the latest details for a given mzone. |
| OPS IP addresses | This is jump host information maintained by Ops. | Contains very useful server info and access info for the DALs, and TD. Does not include POK. |


    
    

## Mzone: 2114




| Owner | Location | Type | Description | Notes |
| --- | --- | --- | --- | --- |
|  | DAL11 | dev | 10 nodes including 2 K8 masters (no local storage), 2 edge, and 2  compute, 2 service and 2 control nodes. | deployers: dev: dal1\-qz2\-sr3\-rk095\-s04 ( IP: 10\.200\.95\.39\) Int: dal1\-qz2\-sr3\-rk095\-s03 (IP: 10\.200\.95\.38\) Slack Channel: <https://ibmcloudlab.slack.com/archives/C04RARG9J4U> |


#### Node Details:








| Node | personality | CPU |
| --- | --- | --- |
| dal1\-qz2\-sr2\-rk203\-s18 | master | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk204\-s18 | master | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk204\-s28 | control | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk204\-s30 | control | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk203\-s20 | edge | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk204\-s20 | edge | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk204\-s32 | service | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk204\-s34 | service | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk203\-s12 | compute | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal1\-qz2\-sr2\-rk203\-s14 | compute | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |


## Mzone: 7203




| Owner | Location | Type | Description | Notes |
| --- | --- | --- | --- | --- |
|  | DAL12 | staging | 5 nodes including 1 K8 master (no local storage), 1 edge node, and 2  compute/control nodes , 1 z node (compute) | deployer: dal2\-qz2\-sr2\-rk219\-s04 (IP 10\.200\.127\.39, 192\.168\.219\.17\)Slack Channel : <https://ibmcloudlab.slack.com/archives/C01A5H6DCCU> |


#### Node Details:




| Node | personality | CPU |
| --- | --- | --- |
| dal2\-qz2\-sr2\-rk118\-s12 | master | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk118\-s11 | compute\-control | Intel(R) Xeon(R) Gold 6142 CPU @ 2\.60GHz |
| dal2\-qz2\-sr2\-rk118\-s13 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk118\-s14 | edge | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk070\-s16 | compute |  |


## Mzone: 7204




| Owner | Location | Type | Description | Notes |
| --- | --- | --- | --- | --- |
|  | DAL12 | staging | 4 nodes including 1 K8 master (no local storage), 1 edge node, and 2  compute/control nodes. | deployer:dal2\-qz2\-sr2\-rk219\-s04 ( IP 10\.200\.127\.39) Slack Channel: <https://ibmcloudlab.slack.com/archives/C019XEDM1H9> |


#### Node Details:




| Node | personality | CPU |
| --- | --- | --- |
| dal2\-qz2\-sr2\-rk118\-s15 | master | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk118\-s17 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk118\-s18 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk118\-s10 | edge | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr9\-rk318\-s38 | compute\-control | Genuine Intel(R) CPU 0000%@ |


## Mzone: 7212




| Owner | Location | Type | Description | Notes |
| --- | --- | --- | --- | --- |
|  | DAL12 | dev | 8 nodes including 1 \+ 1K8 master, no local storage, 1 \+ 1 edge node,  3 x86 compute nodes, 1 POWER9 node. | **RIAS Capable** deployer: dal2\-qz2\-sr2\-rk219\-s04 ( IP 10\.200\.127\.39) Slack Channel: <https://ibmcloudlab.slack.com/archives/CHKK7KCUR> |


#### Node Details:




| Node | personality | CPU |
| --- | --- | --- |
| dal2\-qz2\-sr2\-rk033\-s07 | master | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk115\-s12 | master | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk033\-s08 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk033\-s09 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk033\-s10 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk115\-s16 | compute | Intel(R) Xeon(R) Gold 6142 CPU @ 2\.60GHz |
| dal2\-qz2\-sr2\-rk033\-s11 | edge | Intel(R) Xeon(R) Gold 6142 CPU @ 2\.60GHz |
| dal2\-qz2\-sr2\-rk115\-s15 | edge | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr9\-rk318\-s04 | compute | ??? |


## Mzone: 7213




| Owner | Location | Type | Description | Notes |
| --- | --- | --- | --- | --- |
|  | DAL12 | dev | 4 nodes including 1 K8 master, no local storage, 1 edge node, 2 x86  compute nodes | used mostly for hostos config development and testing     deployer: dal2\-qz2\-sr2\-rk219\-s04 ( IP 10\.200\.127\.39) Slack Channel: <https://ibmcloudlab.slack.com/archives/CHKK6TN49> |


#### Node Details:




| Node | personality | CPU |
| --- | --- | --- |
| dal2\-qz2\-sr2\-rk033\-s12 | master | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk033\-s13 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk033\-s14 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal2\-qz2\-sr2\-rk033\-s15 | edge\-nfs | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |


## Mzone: 7324




| Owner | Location | Type | Description | Notes |
| --- | --- | --- | --- | --- |
|  | DAL13 |  | 5 nodes with  1 K8 master, 2 compute control, one compute (with  local disk) and compute\-edge node | **RIAS Capable**deployer: dal3\-qz2\-sr1\-rk403\-s04 ( IP  10\.200\.143\.39) Slack Channel:<https://ibmcloudlab.slack.com/archives/C0186D0HXCJ> |


#### Node Details:




| Node | personality | CPU |
| --- | --- | --- |
| dal3\-qz2\-sr1\-rk358\-s17 | master | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal3\-qz2\-sr1\-rk331\-s19 | compute\-edge | Intel(R) Xeon(R) Gold 6248 CPU @  2\.50GHz |
| dal3\-qz2\-sr1\-rk331\-s18 | compute\-control | Intel(R) Xeon(R) Gold 6248 CPU @ 2\.50GHz |
| dal3\-qz2\-sr1\-rk358\-s15 | compute\-control | Intel(R) Xeon(R) CPU E5\-2683 v4 @ 2\.10GHz |
| dal3\-qz2\-sr1\-rk331\-s17 | compute | Intel(R) Xeon(R) Gold 6248 CPU @ 2\.50GHz |


## Mzone: 230j




| Owner | Location | Type | Description | Notes |
| --- | --- | --- | --- | --- |
|  | DAL13 |  |  | deployer: dal3\-qz2\-sr1\-rk403\-s04 ( IP  10\.200\.143\.39) Slack Channel: <https://ibmcloudlab.slack.com/archives/C01SK5JTHV4> |


#### Node Details:




| Node | personality | CPU |
| --- | --- | --- |
| dal3\-qz2\-sr1\-rk327\-s01 | master | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal3\-qz2\-sr1\-rk327\-s08 | control\-compute | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal3\-qz2\-sr1\-rk327\-s13 | control\-compute | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal3\-qz2\-sr1\-rk327\-s04 | compute | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal3\-qz2\-sr1\-rk327\-s05 | compute | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal3\-qz2\-sr1\-rk327\-s06 | edge | Intel(R) Xeon(R) Platinum 8260 CPU @ 2\.40GHz |
| dal3\-qz2\-sr1\-rk326\-s14 | compute | ?? |


  


