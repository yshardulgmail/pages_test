---
layout: default
title: Iptables rules validation  HostOS Mzone nodes 
parent: Compliance
---
## Audience

VPC Security and Compliance team. 

## Purpose and scope

This page describes the methods used to enable, update, and maintain network IP layer firewall rules on the HostOS Mzone nodes to ensure that only the desired traffic from networks or systems are able to connect to the Mzone nodes. This does not include firewall implementation enforced at RIAS/Kube/Control plane level. This document also does not cover any firewall installation on Deployment Servers. 

As an organization under IBM Corporate, our project has a limited scope of access controls for which it is responsible. Our project follows IBM Corporate policies in addition to our project's procedures. In the event an access control is administered or owned by the IBM Corporation, our project relies on the larger IBM entity to respond to those audit requests for evidence and interviews. This includes, for example, logical access controls to the IBM network (via IBM Corporate Intranet IDs), telecommuting controls, and physical access to IBM buildings. In addition, environmental controls and physical access controls to hardware assets in data centers are provided by SoftLayer (acquired by IBM in 2013\), which is now known as IBM Cloud. 

## Governing Policy

[Public Cloud PaaS Security Policy \- SC\-Systems and Communication Protection](https://pages.github.ibm.com/ibmcloud/Security/policy/SC-System_and_Communications_Protection.html)

## Definitions

* **Mzone node:** A NextGen (NG)  server running IBM customised HostOS installation. These servers exist inside data centre. node type included are 
	+ Control node
	+ Compute node
	+ Service node
	+ Edge node
	+ Master node
* **Egress:** Outbound traffic
* **iptable :** iptables is a user\-space utility program that allows a system administrator to configure the IP packet filter rules of the Linux kernel firewall.
* **Ingress:** Inbound traffic
* **Ipset:** Companion application of iptables, allows to easily block a set of IP addresses

## Managing Mzone node network rule changes

Mzone node network rule changes are available at centralised location on Github [https://github.ibm.com/cloudlab/hostos\-config\-payloads/blob/master/hostos\-allowed\-ports.yml](https://github.ibm.com/cloudlab/hostos-config-payloads/blob/master/hostos-allowed-ports.yml).

## Ipset/iptables overview

Current hostos policy is deny by default and allow by exception.

This is example of how rules are enforced \-[https://github.ibm.com/cloudlab/hostos\-config\-payloads/blob/master/hostos\-allowed\-ports.yml](https://github.ibm.com/cloudlab/hostos-config-payloads/blob/master/hostos-allowed-ports.yml).

**networkpolicy**

```
listening_ports:
  - description: SSH Server for connections from deploy servers
    protocol: TCP
    ports: 22
    interface: eth0
    allowed_ipsets:
    - deploy_server_ips_192
    roles:
    - all
```

Here we are approving ssh traffic from deployer server running on interface eth0 on port 22 for server ip range 192 using ipsets. 

## Updating new rules

Following process flow is followed to update new rules in our network  policy 



| **Step** | **Role** | **Narrative** |
| --- | --- | --- |
|  |  | **Start HostOS network Policy Update** |
| 1 | Service Team | **Identify new port/ip  requirement for service**Proceed to 2 Create JIRA Issue |
| 2 | Service Team | **Create Jira Issue**Create a new issue in JIRA to request HostOS team to allow new port/ip Proceed to 3 |
| 3 | HostOS Team | **Initial evaluation of new  requirement**Proceed to 4 |
| 4 | Service Team | **Request security Approval from CISO team**Proceed to 5 |
| 5\. | HostOS Team | **Do necessary code changes to update network policy as per details mentioned in Jira ticket.** |
|  |  | **End**HostOS  Network Policy update Procedure is complete |

## Review process

We have quarterly and monthly validation review process.

### Monthly review process

We follow [https://github.ibm.com/ibmcloud/Security/blob/master/architectural_review/least-functionality-report-template.md](https://github.ibm.com/ibmcloud/Security/blob/master/architectural_review/least-functionality-report-template.md)

1. In JIRA open a new **SCE task** in the **CloudLab** project.
2. In the title and description fields, use "Perform <month> Review of Gentor IP Tables Rules"
3. Attach output to SCE task

### Quarterly review process

Quarterly SOC2 control for network rule revalidation requires that we review the firewalld and iptables rules in git and gather information from random  Mzone production node to ensure that rules exist as expected 

The following procedure describes what to include in the JIRA ticket and steps to perform on your local machine:

1. In JIRA open a new **SCE task** in the **CloudLab** project.
2. In JIRA open a new **CIO task** in the **CloudLab** project.
	1. This is used mainly for internal team assignment for quarterly  GitHub and scripts validation.
3. In the title and description fields, use "Perform Second Quarter \<year\> Review of Gentor IP Tables Rules"
4. Get Iptable/ipset command execution output from production Mzone node.
5. Attach output to SCE task

 
## Evidence

| --- | --- | --- |
| 1 | Monthly review process evidence | [SCE\-6632](https://jiracloud.swg.usma.ibm.com:8443/browse/SCE-6632?src=confmacro)  \-  Perform First Quarter 2023 Review of Gentor IP Tables Rules Done   SCE Ticket for Quarterly Validation   [CIO\-8268](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8268?src=confmacro)  \-  1Q 2023 iptables revalidation Closed   CIO Ticket for Quaterly Validation |
| 2 | Quarterly review process evidence | [SCE\-3692](https://jiracloud.swg.usma.ibm.com:8443/browse/SCE-3692?src=confmacro)  \-  Perform NOV 2024 Review of Gentor IP Tables Rules Done |


## Enforcement

* Company executives, leaders, and senior management are required to ensure that internal audit mechanisms exist to monitor and measure compliance with this procedure.
* Managers and senior managers have the responsibility to enforce compliance with the procedure.




 


