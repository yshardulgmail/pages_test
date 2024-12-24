---
layout: default
title: Release Bundles 
parent: Architecture
---
## Operating System Support

HostOS currently offers and deploys Ubuntu 18:04 as the primary operating system on all IBM Nextgen VPC datacenters.

HostOS currently supports 3 different kernel versions for Ubuntu 18:04 deployments and provides a separate set of release bundles for each version:

1. HostOS Release 2\.x with ibm gt linux kernel 4\.15\.0\-1069\-ibm\-gt
2. HostOS Release 3\.x with ibm gt linux kernel 4\.15\.0\-1080\-ibm\-gt
3. HostOS Release 5\.x with ibm gt linux kernel 4\.15\.0\-1108\-ibm\-gt

  


## Platform Support

HostOS supports deployment for the following machine platforms:

* amd64 (x86\-64\)
* s390x (IBM System Z)

  


## Design

HostOS provides several Release Bundles to perform boot, configuration and setup of applications on a system before it can be used for deploying overlying IBM Nextgen VPC components for Kube, Genctl and others

As described earlier, every HostOS Release Bundle is a docker image that packages the following components:

1. **Payload**  
This includes OS Artifacts such as kernel, initrd, root filesystem, application Debian packages and configuration scripts
2. **Tools**  
Python based tooling to deploy the above components to a given system

  


The above components are collectively packaged as a Docker image which is then fed to the DDT/MDS tool to initiate the deployment process.  
Deployment and application secrets if any are referenced through Hashicorp Vault Instances

  


## Release Bundle Classification

Following are the available HostOS Release bundles:

## 1\. hostos\-boot\-release

As the name suggests this is the Release bundle responsible for booting a given system with the ibm gt linux kernel and provide the bare minimum dependencies needed to support deployment of the next set of bundles.

**Sources:**

Release Bundle: [https://github.ibm.com/cloudlab/hostos\-boot\-release](https://github.ibm.com/cloudlab/hostos-boot-release)  
Tools: [https://github.ibm.com/cloudlab/hostos\-boot\-tools](https://github.ibm.com/cloudlab/hostos-boot-tools)  
Payload: [https://github.ibm.com/cloudlab/hostos\-boot\-payloads](https://github.ibm.com/cloudlab/hostos-boot-payloads)

  
**Some prominent characteristics:**

* The Boot release tool mainly uses [xcat](https://xcat-docs.readthedocs.io/en/stable/index.html) for booting the physical machines over network. The 5\.x release version uses simpleboot tool to deploy s390x platform systems and xcat for amd64 platform
* The Boot release payload mainly comprises of the kernel, rootfs and initrd files
* The Boot release packages payloads for both amd64 and s390x platforms and the Boot tool decides the correct payload to be used for booting based on the machine platform
* All HostOS booted systems use ramfs based filesystems and are volatile in nature. This means that rebooting or redeploying the Boot Release will destroy the previous deployed instance and as such we need to be very careful with boot deployments

  


## 2\. hostos\-config\-release

The Config release mainly deals with configuration for system, network, storage, compute, security and many other application components.

The Config payload has a large collection of shell\-based config scripts for different components and uses the following criteria to pick the required scripts for execution:

* Physical machine personality type: Personality refers to type of a physical machine/node in an mzone and may include a compute,service,master and other classifications
* Deployment stage: can be one of pre\-config,pre\-install, post\-install or post\-config
* Mzone node platform architecture
* Feature flags
* Some scripts also track the following characteristics before execution:  
\- Config script changes across versions  
\- Debian package version changes for application specific scripts  
\- Mzone Undercloud file config updates

The config payload maintains a [yml config](https://github.ibm.com/cloudlab/hostos-config-payloads/blob/master/hostos-config.yml) file that defines the eligibility of the scripts execution across several criteria’s as described above.

In addition to application/system configuration, the Config release is also responsible for handling Mzone Expansions to configure newly added nodes to be correctly added in the existing mzone cluster

**Sources:**

Release Bundle: [https://github.ibm.com/cloudlab/hostos\-config\-release](https://github.ibm.com/cloudlab/hostos-config-release)  
Tool: [https://github.ibm.com/cloudlab/hostos\-config\-tools](https://github.ibm.com/cloudlab/hostos-config-tools)  
Payload: [https://github.ibm.com/cloudlab/hostos\-config\-payloads](https://github.ibm.com/cloudlab/hostos-config-payloads)

  
**Some prominent characteristics:**

* The Config payload has a verify script associated with each config script. The verify script is used to check configuration status before and after the associated config script is executed.It also serves a verification script for dry run based deployments
* The Config scripts always generates and maintains a hash of the current scripts which are used to track scripts across future deployments for any updates.Scripts are only executed in most cases when there is script hash mismatch which indicates a script update. The hashes are stored under the path ***/etc/genesis/***
* The config payload maintains the source of truth based config files thats used by many hostos monitoring tools for ipset,iptables,process and configuration monitoring plugins

  


## 3\. hostos\-base\-os\-sw\-release

The base\-os\-sw release is responsible for providing Debian application packages and its dependencies to support the  following Cloud components for IBM Nextgen VPC:

* Hypervisor, Compute (QEmu,libvirt)
* Storage
* Network
* Security
* Kubernetes
* Monitoring Utilities
* Development Utilities

The base\-os\-sw payload maintains a collection of Debian packages for all supported platforms.Majority of the packages installed through base\-os\-sw is provided by Canonical and few are from third\-party vendors.

Similar to config release, the base\-os\-sw also maintains a [yml file](https://github.ibm.com/cloudlab/hostos-upgrade-payloads/blob/master/manifests/base-os-sw-3.x.yml) based manifest to track criteria required to install packages on a given node.

**Sources:**

Release: [https://github.ibm.com/cloudlab/hostos\-base\-os\-sw\-release](https://github.ibm.com/cloudlab/hostos-base-os-sw-release)  
Tool: [https://github.ibm.com/cloudlab/hostos\-upgrade\-tools](https://github.ibm.com/cloudlab/hostos-upgrade-tools)  
Payload: [https://github.ibm.com/cloudlab/hostos\-upgrade\-payloads](https://github.ibm.com/cloudlab/hostos-upgrade-payloads)  
  


**Some prominent characteristics:**

* Base\-os\-sw payload can also be used to remove any unwanted packages from the system or a specific version of a package
* Base\-os\-sw release sets up a local Signed Debian repository on a mzone node under deployment and uses it as the source to install the required packages
* Base\-os\-sw release maintains packages for am64 and s390x platforms and the tool decides which payload to use accordingly

  


## 4\. hostos\-base\-net\-sw\-release

This bundles functions similar to base\-os\-sw but is designed to mainly provide ibm built Debian packages for SDN systems  
The base\-net\-sw release also runs some custom config scripts for sdn configuration.

**Sources:**

Release: [https://github.ibm.com/cloudlab/hostos\-base\-net\-sw\-release](https://github.ibm.com/cloudlab/hostos-base-net-sw-release)  
Tool: [https://github.ibm.com/cloudlab/hostos\-upgrade\-tools](https://github.ibm.com/cloudlab/hostos-upgrade-tools)  
Payload: [https://github.ibm.com/cloudlab/hostos\-upgrade\-payloads](https://github.ibm.com/cloudlab/hostos-upgrade-payloads)

  


## 5\. hostos\-nextgen\-os\-sw\-release

This bundle functions similar to base\-os\-sw but is designed to mainly provide ibm built Debian packages for Genctl components that installs a system agent on HostOS for managing compute, network, storage and security resources.  
The system agents installed by this release is mainly used by the overlying genctl service pods hosted on the Genctl Kubernetes cluster running in a HostOS Mzone  
  


Nextgen payload also bundles HostOS plugins that are used to monitor and audit system iptables, ipset , process and configuration changes

**Sources:**

Release: [https://github.ibm.com/cloudlab/hostos\-nextgen\-os\-sw\-release](https://github.ibm.com/cloudlab/hostos-nextgen-os-sw-release)  
Tool: [https://github.ibm.com/cloudlab/hostos\-upgrade\-tools](https://github.ibm.com/cloudlab/hostos-upgrade-tools)  
Payload: [https://github.ibm.com/cloudlab/hostos\-upgrade\-payloads](https://github.ibm.com/cloudlab/hostos-upgrade-payloads)

  


## 6\. hostos\-kernel\-patch\-release

This release bundle is used to apply canonical live kernel patch updates to a given system.  
Canonical provides separate patches for different kernel versions that HostOS supports

**Sources:**

Release: [https://github.ibm.com/cloudlab/hostos\-kernel\-patch\-release](https://github.ibm.com/cloudlab/hostos-kernel-patch-release)  
Tool: [https://github.ibm.com/cloudlab/hostos\-kernel\-patch\-tools](https://github.ibm.com/cloudlab/hostos-kernel-patch-tools)  
Payload: [https://github.ibm.com/cloudlab/hostos\-kernel\-patch\-payloads](https://github.ibm.com/cloudlab/hostos-kernel-patch-payloads)

  


## 7\. hostos\-validate\-release

The Validate Release Bundle is responsible for executing automated tests on HostOS Mzones to check various functionality and system configuration  
This is a non\-standard HostOS release bundle as its not deployed as part of a regular deployment.  
  


It has been designed to be platform and HostOS release version independent and can be deployed against any system to validate the associated tests.  
  


The Validate payload is a collection of python based scripts that execute a set of tests on a given mzone node and provide the reports for verification.

SRB: <https://github.ibm.com/cloudlab/srb/blob/master/architecture/hostos/payloads/host_validate.md>

**Sources:**

Release: [https://github.ibm.com/cloudlab/hostos\-validate\-release](https://github.ibm.com/cloudlab/hostos-validate-release)  
Tool: [https://github.ibm.com/cloudlab/hostos\-validate\-tools](https://github.ibm.com/cloudlab/hostos-validate-tools)  
Payload: [https://github.ibm.com/cloudlab/hostos\-validate\-payloads](https://github.ibm.com/cloudlab/hostos-validate-payloads)

