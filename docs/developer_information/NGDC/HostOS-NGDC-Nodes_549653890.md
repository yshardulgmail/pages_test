---
layout: default
title: NGDC Nodes 
parent: NGDC
grand_parent: Developer Information
---

HostOS Has access to these NGDC Nodes. HostOS team members may use any available node for NGDC development.

1. Put your name down in the "In use by ... until" column and specify how long you need it for (for multi\-days reservation.)
2. For a quick reservation (\<24 hours) please announce in \#hostos\-on\-ngdc\-dev channel which node you are using. Always check in the slack channel before taking a node.
3. **It should be understood that if you're working on something HOT that can't be interrupted, then a proper reservation on a node is expected. If the "In use by ... until" date has passed, then it's fair for other to claim the node.**
4. Remember that these are test machines, always backup your work elsewhere.

`warning` indicates known issue with node.
`Golden star` indicates known working node according to the provision files.

  


[Access to NGDC environment and FAST Provisioning](https://confluence.swg.usma.ibm.com:8445/display/HCP/HostOS+Provisioning+on+NGDC+Nodes)* Tickets requesting hardware: [NEXT\-196](https://jiracloud.swg.usma.ibm.com:8443/browse/NEXT-196), [NEXT\-197](https://jiracloud.swg.usma.ibm.com:8443/browse/NEXT-197), NEXT\-198



| Node VPC Name | Provision files | Notes | In use by... until |
| --- | --- | --- | --- |
| ElbaCtrl: VPCs10 | * Role: vpc\_elba\_zone\_control\_sdn\_host\_test\_hostos * Undercloud: ngdc\_dal09\-qz4\-undercloud\-internal\-nessus\-hostos | * nessus connected sysop@dal2\-qz4\-sr7\-rk023\-s10:\~$ cat /etc/genesis/\*release\*hostos\-boot\-release:6\.3\.3\-20240530T201009Z\_619216f (Jun 21 2024 20:33:19 UTC, STATUS\=success)hostos\-config\-release:6\.1\.45\-20240611T120431Z\_acc49f8 (Jun 21 2024 20:33:45 UTC, STATUS\=success, EXPANSION\=False)hostos\-base\-os\-sw\-release:6\.1\.34\-20240612T053123Z\_6f3a010 (Jun 21 2024 20:35:45 UTC, STATUS\=success)hostos\-base\-net\-sw\-release:6\.13\.21\-20240531T035808Z\_2e04b09 (Jun 21 2024 20:36:14 UTC, STATUS\=success)basenet\-config\-release:6\.11\.22\-20240611T042643Z\_8791a39 (Jun 21 2024 20:36:34 UTC, STATUS\=success, EXPANSION\=None)hostos\-nextgen\-os\-sw\-release:6\.1\.29\-20240612T100642Z\_f9bb4d0 (Jun 21 2024 20:37:23 UTC, STATUS\=success)hostos\-config\-release:6\.1\.45\-20240611T120431Z\_acc49f8 (Jun 21 2024 20:42:27 UTC, STATUS\=success, EXPANSION\=False) |  |
| glowing star ElbaCtrl: VPC[dal09\-qz4\-sr07\-](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/43392/)**[rk23](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/206/)**\-s12 | * Role: vpc\_elba\_zone\_control\_sdn\_host\_test, [hostos\_vpc\_zone\_ctrl\_smart\_nic](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/108/) * Undercloud: ngdc\_dal09\-qz4\-undercloud\-internal\-nessus\-hostos, [dal4\-qz4\-undercloud\-hostos](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) * uuid: add9c43e\-d2e6\-4e38\-b9e3\-0c733e2852f1 * IP: 192\.168\.23\.24; 198\.18\.4\.35 | Last provisioned: 6/21/2024 nessus scannedsysop@dal2\-qz4\-sr7\-rk023\-s12:\~$ cat /etc/genesis/\*release\*hostos\-boot\-release:6\.3\.3\-20240530T201009Z\_619216f (Jun 21 2024 20:03:26 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 21 2024 20:03:51 UTC, STATUS\=success, EXPANSION\=False)hostos\-base\-os\-sw\-release:6\.1\.33\-20240522T151510Z\_ec61009 (Jun 21 2024 20:05:56 UTC, STATUS\=success)hostos\-base\-net\-sw\-release:6\.13\.21\-20240531T035808Z\_2e04b09 (Jun 21 2024 20:06:25 UTC, STATUS\=success)basenet\-config\-release:6\.11\.22\-20240611T042643Z\_8791a39 (Jun 21 2024 20:06:44 UTC, STATUS\=success, EXPANSION\=None)hostos\-nextgen\-os\-sw\-release:6\.1\.28\-20240606T133718Z\_bb5cf63 (Jun 21 2024 20:07:33 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 21 2024 20:13:03 UTC, STATUS\=success, EXPANSION\=False)hostos\-config\-release:6\.1\.45\-20240611T120431Z\_acc49f8 (Jun 21 2024 20:20:30 UTC, STATUS\=success, EXPANSION\=False) | \#hostos\-on\-ngdc\-dev |
| ElbaCtrl: VPCs36 CI pipeline | * Role: vpc\_elba\_zone\_control\_sdn\_host\_test * Undercloud: ngdc\_dal09\-qz4\-undercloud\-internal\-nessus\-hostos | * last provisioned: 6/21/2024 nessus scanedroot@dal2\-qz4\-sr7\-rk023\-s36:\~\# cat /etc/genesis/\*release\*hostos\-boot\-release:6\.3\.3\-20240530T201009Z\_619216f (Jun 20 2024 23:13:40 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 20 2024 23:14:06 UTC, STATUS\=success, EXPANSION\=False)hostos\-base\-os\-sw\-release:6\.1\.33\-20240522T151510Z\_ec61009 (Jun 20 2024 23:16:08 UTC, STATUS\=success)hostos\-base\-net\-sw\-release:6\.13\.21\-20240531T035808Z\_2e04b09 (Jun 20 2024 23:16:38 UTC, STATUS\=success)basenet\-config\-release:6\.11\.22\-20240611T042643Z\_8791a39 (Jun 20 2024 23:16:58 UTC, STATUS\=success, EXPANSION\=None)hostos\-nextgen\-os\-sw\-release:6\.1\.28\-20240606T133718Z\_bb5cf63 (Jun 20 2024 23:17:48 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 20 2024 23:23:31 UTC, STATUS\=success, EXPANSION\=False)hostos\-config\-release:6\.1\.45\-20240611T120431Z\_acc49f8 (Jun 21 2024 19:33:48 UTC, STATUS\=success, EXPANSION\=False) |  |
| glowing star ElbaCtrl: VPC[dal09\-qz4\-sr07\-**rk23**\-s38](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/43402/) | * Role: vpc\_elba\_zone\_control\_sdn\_host\_test, [hostos\_vpc\_zone\_ctrl\_smart\_nic](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/108/) (older) * Undercloud: ngdc\_dal09\-qz4\-undercloud\-internal\-nessus\-hostos, [dal4\-qz4\-undercloud\-hostos](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) (older) * uuid: 2d062065\-8bfd\-48d9\-a7b0\-9fb8c9583a84 * IP: 192\.168\.23\.76; 198\.18\.4\.30 | * Last provisioned 6/19/2029 nessus scannedsysop@dal2\-qz4\-sr7\-rk023\-s38:\~$ cat /etc/genesis/\*release\*hostos\-boot\-release:6\.3\.3\-20240530T201009Z\_619216f (Jun 19 2024 21:21:22 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 19 2024 21:21:47 UTC, STATUS\=success, EXPANSION\=False)hostos\-base\-os\-sw\-release:6\.1\.33\-20240522T151510Z\_ec61009 (Jun 19 2024 21:23:49 UTC, STATUS\=success)hostos\-base\-net\-sw\-release:6\.13\.21\-20240531T035808Z\_2e04b09 (Jun 19 2024 21:24:18 UTC, STATUS\=success)basenet\-config\-release:6\.11\.22\-20240611T042643Z\_8791a39 (Jun 19 2024 21:24:37 UTC, STATUS\=success, EXPANSION\=None)hostos\-nextgen\-os\-sw\-release:6\.1\.28\-20240606T133718Z\_bb5cf63 (Jun 19 2024 21:25:26 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 19 2024 21:30:19 UTC, STATUS\=success, EXPANSION\=False)hostos\-config\-release:6\.1\.45\-novlibvirt\-20240619T210249Z\_acc49f8 (Jun 19 2024 22:30:25 UTC, STATUS\=success, EXPANSION\=False) | \#hostos\-on\-ngdc\-dev |
| glowing star ElbaCtrl: VPC[dal09\-qz4\-sr07**\-rk24**\-s12](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/43608/) | * Role: vpc\_elba\_zone\_control\_sdn\_host\_test * Undercloud: ngdc\_dal09\-qz4\-undercloud\-internal\-nessus\-hostos * uuid: 78837e68\-dff2\-42c4\-9dbf\-0b4711cf02cc * IP: 192\.168\.24\.24; 198\.18\.4\.89 | * + Last provisioned: 6/21/2024 * sysop@dal2\-qz4\-sr7\-rk024\-s12:\~$ cat /etc/genesis/\*release\*hostos\-boot\-release:6\.3\.3\-20240530T201009Z\_619216f (Jun 21 2024 18:58:42 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 21 2024 18:59:08 UTC, STATUS\=success, EXPANSION\=False)hostos\-base\-os\-sw\-release:6\.1\.33\-20240522T151510Z\_ec61009 (Jun 21 2024 19:01:10 UTC, STATUS\=success)hostos\-base\-net\-sw\-release:6\.13\.21\-20240531T035808Z\_2e04b09 (Jun 21 2024 19:01:40 UTC, STATUS\=success)basenet\-config\-release:6\.11\.22\-20240611T042643Z\_8791a39 (Jun 21 2024 19:02:01 UTC, STATUS\=success, EXPANSION\=None)hostos\-nextgen\-os\-sw\-release:6\.1\.28\-20240606T133718Z\_bb5cf63 (Jun 21 2024 19:02:50 UTC, STATUS\=success)hostos\-config\-release:6\.1\.44\-20240605T150537Z\_d38ec8f (Jun 21 2024 19:08:53 UTC, STATUS\=success, EXPANSION\=False)hostos\-config\-release:6\.1\.45\-20240611T120431Z\_acc49f8 (Jun 21 2024 19:19:03 UTC, STATUS\=success, EXPANSION\=False) |  |
| glowing star ElbaCompute: VPC[dal2\-qz4\-sr7\-**rk024**\-s38](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/43613/) | * Role: vpc\_elba\_zonelet\_compute\_hostos * Undercloud: ngdc\_dal09\-qz4\-undercloud\-hypervisor * uuid: b5c1c596\-ac86\-4e4e\-9fa4\-5c62d8f24557 * IP: 192\.168\.24\.76; 198\.18\.16\.5 | * 5/6/2024: failed to install nextgen: [FAB\-19475](https://jiracloud.swg.usma.ibm.com:8443/browse/FAB-19475) resolved! * sysop@dal2\-qz4\-sr7\-rk024\-s38:\~$ cat /etc/genesis/\*release\*hostos\-boot\-release:6\.3\.2\-20240312T144633Z\_508fb59 (May 09 2024 01:30:48 UTC, STATUS\=success)hostos\-config\-release:6\.1\.38\-ngdc1\-20240429T193956Z\_e18b278 (May 09 2024 01:41:29 UTC, STATUS\=success, EXPANSION\=False)hostos\-base\-os\-sw\-release:6\.1\.31\-20240425T040619Z\_ba74350 (May 09 2024 01:54:02 UTC, STATUS\=success)hostos\-base\-net\-sw\-release:6\.12\.75\-20240409T125323Z\_7341e69 (May 09 2024 02:04:46 UTC, STATUS\=success)basenet\-config\-release:6\.11\.22\-20240508T100541Z\_42b3958 (May 09 2024 02:17:32 UTC, STATUS\=success, EXPANSION\=None)hostos\-nextgen\-os\-sw\-release:6\.1\.26\-20240427T141419Z\_ba6db26 (May 09 2024 02:30:04 UTC, STATUS\=success)hostos\-config\-release:6\.1\.38\-ngdc1\-20240429T193956Z\_e18b278 (May 09 2024 02:42:57 UTC, STATUS\=failed, EXPANSION\=False)hostos\-config\-release:6\.1\.38\-ngdc1\-20240429T193956Z\_e18b278 (May 09 2024 03:23:39 UTC, STATUS\=success, EXPANSION\=False) |  |
| Elba Control on rk24? for CI Pipeline: TBD |  |  |  |
| Elba Compute: VPC **CI Pipeline**[dal09\-qz4\-sr07\-rk24\-s40](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/43621/)Do not use this for development, CI pipeline only. |  | * 5/9/2024  sysop@dal2\-qz4\-sr7\-rk024\-s40:\~$ cat /etc/genesis/\*release\*hostos\-boot\-release:6\.3\.2\-20240312T144633Z\_508fb59 (May 09 2024 21:58:22 UTC, STATUS\=success)hostos\-config\-release:6\.1\.38\-ngdc1\-20240429T193956Z\_e18b278 (May 09 2024 22:09:04 UTC, STATUS\=success, EXPANSION\=False)hostos\-base\-os\-sw\-release:6\.1\.31\-20240425T040619Z\_ba74350 (May 09 2024 22:21:37 UTC, STATUS\=success)hostos\-base\-net\-sw\-release:6\.12\.75\-20240409T125323Z\_7341e69 (May 09 2024 22:32:21 UTC, STATUS\=success)basenet\-config\-release:6\.11\.22\-20240508T100541Z\_42b3958 (May 09 2024 22:45:08 UTC, STATUS\=success, EXPANSION\=None)hostos\-nextgen\-os\-sw\-release:6\.1\.26\-20240427T141419Z\_ba6db26 (May 09 2024 22:57:41 UTC, STATUS\=success)hostos\-config\-release:6\.1\.38\-ngdc1\-20240429T193956Z\_e18b278 (May 09 2024 23:14:55 UTC, STATUS\=success, EXPANSION\=False)hostos\-config\-release:6\.1\.38\-cio10829\-20240509T220610Z\_e18b278 (May 09 2024 23:48:33 UTC, STATUS\=success, EXPANSION\=False) | Do not use this. For CI only. |
| Elba Control: bx3d: rk56 for **CI Pipeline** [dal09-qz4-sr07-rk56-s36](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/44477/)|  | | |
| Elba Compute: mx3d: rk56 for **CI Pipeline** [dal09-qz4-sr07-rk56-s34](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/44476/)| | | |
| Elba Control: bx3d: rk56 for dev [dal09-qz4-sr07-rk56-s06](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/44466/) | * uuid: b223c158\-c7d0-495a\-baa2\-bd7b20c6bd73 * DPU Role: [ngdc-dal10-qz5-compute-zonelet-control-node-dpu](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/749/) * DPU Undercloud: [ngdc_dal09-qz4-undercloud-backend](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/254/) * HostOS Role: [ngdc-dal10-qz5-compute-zonelet-control-node-hostos-role](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/748/) * HostOS Undercloud: [ngdc_dal09-qz4-undercloud-hypervisor](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/250/) | | |
| Elba Compute: bx3d: rk56 for dev [dal09-qz4-sr07-rk56-s38](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/44478/)| * uuid: 876c674e\-56d2-4ac7\-b21e\-13bdfc2f96a9 * DPU Role: [ngdc-dal10-qz5-compute-node-dpu](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/750/) * DPU Undercloud: [ngdc_dal09-qz4-undercloud-backend](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/254/) * HostOS Role: [ngdc-dal10-qz5-compute-node-hostos-role](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/588/) * HostOS Undercloud: [ngdc_dal09-qz4-undercloud-hypervisor](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/250/) | | |
| warning CX\-6 \- Edge: VPC[dal09\-qz4\-sr07\-rk45\-s36](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/18179/) | * tenant: vpc * Role: [ngdc-dal10-qz5-edge-edge-node-hostos-role](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/590/) * Undercloud: [ngdc_dal09-qz4-undercloud-backend-rk24](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/378/) * uuid: 3ba27dac\-3ee7\-4e68\-a616\-f017aa0754c9 * IP: 192\.168\.45\.72 | same issue as rk45 s04\.recently used by Rick (6\.6\.318\-rm\-20240501T180544Z\_0b28756\) | \#hostos\-on\-ngdc\-dev |
| warning CX\-6 \- Edge: VPC[dal09\-qz4\-sr07\-rk45\-s34](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/14559/) | * Role: [ngdc-dal10-qz5-edge-edge-node-hostos-role](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/590/) * Undercloud: [ngdc_dal09-qz4-undercloud-backend-rk24](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/378/) * uuid: 34e790f8\-bdde\-4133\-9e83\-1d07e59d3ff3 * IP: 192\.168\.45\.68 | [Reported issue to Hitesh](https://ibm-cloudplatform.slack.com/archives/C03N1PERU4S/p1709172243592659?thread_ts=1708717385.054089&cid=C03N1PERU4S) (basenet is working now)can't ping nessus [NFS\-17651](https://jiracloud.swg.usma.ibm.com:8443/browse/FAB-17651) | Dany and Hillol Chakraborty3/31/2024 |
| Elba Mgmt: TBD? |  | Need outlook for these.  Grant to talk to Ryan. |  |
| Elba Mgmt: TBD? |  | Need outlook for these.  Grant to talk to Ryan. |  |
| glowing star CX\-5 Mgmt: Fabric [dal09\-qz4\-sr07\-rk48\-s04](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/42576/)(aka: k8s\-worker\-01\-b) | * Role: [hostos\_uc\_k8s\_mgmt](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/38/) * Undercloud: [dal4\-qz4\-undercloud\-hostos](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) * uuid: 9a13dff4\-a0da\-4b5f\-a2c0\-67a3de8c5a44 * IP: 198\.19\.0\.22; 198\.18\.0\.22 | Loan from Rizwan | Dany (kube cluster)until 5/30/2024 |
| glowing starCX\-5 Mgmt: Fabric [dal09\-qz2\-sr07\-rk046\-sl49](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/42575/)(aka: k8s\-worker\-01\-f) | * Role: [hostos\_uc\_k8s\_mgmt](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/38/) * Undercloud: [dal4\-qz4\-undercloud\-hostos](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) * uuid: 78605977\-9ce1\-4399\-af8b\-05bcf897e4b3 * IP: 198\.19\.0\.11; 198\.18\.0\.11 | last provisioned 3/13Loan from Josehostname changing issues: [1](https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1711065556136159?thread_ts=1711040562.091699&cid=C01L963NZ1D) | \#hostos\-on\-ngdc\-dev |
| glowing star Mcdvm: Fabric[hostos\-test\-new](https://us-east.dcim.test.cloud.ibm.com/virtualization/virtual-machines/220/) | * Role: [ncs\_skip\_hostos](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/37/) * Undercloud: [dal4\-qz4\-undercloud\-vmaas](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/100/) * uuid: 302fdbae\-2015\-4ec8\-9cba\-c6dcd0eb9b17 * IP: 198\.18\.3\.154 |  | Dany \- 3/26/2024 |
| glowing star Mcdvm: Fabric[hostos\-multi\-net](https://us-east.dcim.test.cloud.ibm.com/virtualization/virtual-machines/320/interfaces/) | * Role: [ncs\_skip\_hostos](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/164/) * Undercloud: [uc\_qz4\_services\_feddev](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/153/) * uuid:  680d7e27\-9f2f\-4e8c\-9c4a\-c15710fd63aa * IP: 198\.18\.3\.108 |  | Sujitha \- 03/29/2024 |

* Common issues:  
bmc not pingping: clone this [Jira ticket](https://jiracloud.swg.usma.ibm.com:8443/browse/NFP-175)

  


Jira tickets:



| Priority | Ticket number |  |  |
| --- | --- | --- | --- |
| Blocking |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
| High |  |  |  |
|  |  |  |  |
|  |  |  |  |
| Medium |  |  |  |
|  |  |  |  |
|  |  |  |  |

  




|  | mcdvm | mgmt | edge | control | compute |
| --- | --- | --- | --- | --- | --- |
| sysdig agent | tested | tested |  | can't ping |  |
| fim | tested | tested |  |  |  |
| vault | tested | tested |  |  |  |
| fluentbit |  |  |  |  |  |
| iptables |  |  |  |  |  |
| ldap |  |  |  |  |  |
| EDR | tested |  |  |  |  |
| Nessus | tested |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |

  




 


Document generated by Confluence on Jul 15, 2024 13:04


[Atlassian](https://www.atlassian.com/)


 


