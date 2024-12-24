---
layout: default
title: HostOS Automated Testing 
parent: Validations  
---





## HostOS Automated Testing

This page is to provide information of various automated pipelines and tools used for HostOS testing and also to compare its features against that offered by Genctl Concourse CI pipelines.

We would also like to suggest and track areas in HostOS that may be candidates for future automation testing.

## HostOS Testing Enhancements

### **Networking:**

* And SDN tests ? Example: <https://github.ibm.com/cloudnet/fabcon/tree/master/src/fabconapi/cli/examples.v3>
* Network connectivity to different endpoints may already be tested by config

### **Storage:**

Any storage tests?

### **Logging and monitoring tests:**

* Testing process\-monitoring and iptables\-monitoring to ensure no invalid records
* any rsyslog config tests ?
* any auditd/ journaling config checks?

### **Package tests**

To be handled by HostOS deploy testing pipeline  
Example: [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/job/test\_hp\_master/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/job/test_hp_master/)

### **Personality verification checks**

### **Checking cron jobs**

### **FIPS verification checks**

[https://github.ibm.com/cloudlab/hostos\-validate\-payload/pull/17](https://github.ibm.com/cloudlab/hostos-validate-payload/pull/17)

### **Nextgen tools and utils**

Tests for any of the nextgen tools ?

### **kernel module checks?**

modules installed / allowed

### **Container runtime checks**

Docker and containerd

### **Services**

checking service statuses for important ones

  


## **Comparison of Genesis Concourse CI and proposed HostOS Test pipelines**

  




| **Feature Group** | **Feature** | **Genesis Concourse CI\-CD  Pipeline** | **HostOS Test Coverage \*** |
| --- | --- | --- | --- |
| Fresh Deploy | 3x Fresh Deploy | Yes | Yes (VE and Physical Mzone) |
| 5x Fresh Deploy | Yes | Yes (VE and Physical Mzone) |
| 6x Fresh Deploy | Yes | Yes(Physical Mzone Only) |
| Upgrade Deploy | 3x Upgrade Deploy | No | Yes (VE and Physical Mzone) |
| 5x Upgrade Deploy | No | Yes (VE and Physical Mzone) |
| 6x Upgrade Deploy | No | Yes(Physical Mzone Only) |
| Kube, Genctl and RIAS | Kube, Genctl, RIAS Deploy | Yes | Yes (Physical Mzone Only) |
| RIAS Smoke | Yes | Yes (Physical Mzone Only) |
| Platform support | amd64 platform deployment | Yes | Yes (VE and Physical Mzone) |
| S390x platform deployment | Yes | Yes (Physical Mzone 7203 only) |
| Experimental features and configuration | HostOS Test/Experimental Bundle deployment | No | Yes |
| Mzone inventory customization | No | Yes(Allowed, but needs to be supported via automation) |
| HostsOS Validate Release Bundle Tests | Generic Test \-Personality checks | Yes (If validate bundle is deployed) | Yes (VE and Physical Mzone) |
| VSI lifecycle tests (Ubuntu, RHEL, CentOS, Windows) | Yes (If validate bundle is deployed) | Yes (Physical Mzone Only) |
| VSI storage operation tests | Yes (If validate bundle is deployed) | Yes (Physical Mzone Only) |
| VSI snapshot tests | Yes (If validate bundle is deployed) | Yes (Physical Mzone Only) |
| User and Group list verification checks | Yes (If validate bundle is deployed) | Yes (VE and Physical Mzone) |
| Apparmor Tests | Yes (If validate bundle is deployed) | Yes (VE and Physical Mzone) |
| Reporting and Log collection | HTML Test Reporting | No | Yes (VE and Physical Mzone) |
| Audit Data Collection | No | Yes (VE and Physical Mzone) |
| Slack Notification | No | Yes (VE and Physical Mzone) |

\* HostOS test coverage consists of Virtual Environment and Physical mzones

  


### HostOS Test Automation for DAL\-QZ2 test mzones

We plan to follow and utilize the [ops\-container\-mgmt](https://github.ibm.com/cloudlab/ops-container-mgmt) framework for provisioning a hostos jenkins build agent on the dal qz2 deployers.  
This will enable access to Hostos taas Jenkins CI to automate and execute deployment tests on its tests mzones

**Tracking ticket:**  


[![](https://jiracloud.swg.usma.ibm.com:8443/secure/viewavatar?size=xsmall&avatarId=10300&avatarType=issuetype)SRE\-6633](https://jiracloud.swg.usma.ibm.com:8443/browse/SRE-6633?src=confmacro)
 \-
 Assist HostOS team to build their container image for automation
To Do

  


  


**Blockers/Pending tickets:**

Access to physical mzones from HostOS Jenkins CI tool is currently limited and is dependent on following network proxy/firewall enablement tickets:



[![](https://jiracloud.swg.usma.ibm.com:8443/secure/viewavatar?size=xsmall&avatarId=10300&avatarType=issuetype)SRE\-6430](https://jiracloud.swg.usma.ibm.com:8443/browse/SRE-6430?src=confmacro)
 \-
 Access from Deploy Server to Jenkins Instance on IBM Corporate
To Do





[![](https://jiracloud.swg.usma.ibm.com:8443/images/icons/issuetypes/story.svg)SRE\-6595](https://jiracloud.swg.usma.ibm.com:8443/browse/SRE-6595?src=confmacro)
 \-
 New Firewall Rule to allow access to HostOS taas jenkins
Closed




