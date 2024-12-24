---
layout: default
title: Ubuntu 24.04 Releases 
parent: Release Engineering
---


## HostOS on Ubuntu 24.04 (For ELBA DPU Systems Only)

## Development Release Bundles

**Kernel:** 6.8.0-1007-ibm-gt

**Libvirt:** 10.0.0-2ubuntu8.2

**Qemu**: 1:8.2.2+ds-0ubuntu1

**Pensando Ionic Driver**: 24.05.1-001

These are located in "**production**" artifactory

## Standard release bundles

| Release Bundle | Version |
| --- | --- |
| hostos-boot-release | 7.0.0-20240715T154422Z_679373e |
| hostos-config-release | 7.0.0-20240715T194825Z_9716e40 |
| hostos-base-os-sw-release | 7.0.0-20240715T164437Z_0c86f58 |
| hostos-base-net-sw-release | 7.0.0-20240715T152526Z_f8bfe27 |
| hostos-nextgen-os-sw-release | 7.0.0-20240715T153820Z_b531881 |

These are located in "**sandbox**" artifactory - [docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local](http://docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local)

TDX release bundles **(kernel:** **6.8.0-1004-intel with custom libvirt/qemu)**

| Release Bundle | Version |
| --- | --- |
| hostos-boot-release | 7.0.0-20240522T200423Z_9a77d1a |
| hostos-config-release | 7.0.0-20240522T211747Z_293dcc0 |
| hostos-base-os-sw-release | 7.0.0-20240522T173156Z_c741ed7 |
| hostos-base-net-sw-release | 7.0.0-20240511T115328Z_e0723dd |
| hostos-nextgen-os-sw-release | 7.0.0-20240513T105906Z_e2ddc8c |

Working example `MZONE.hjson` file:

```bash
{

  components: {

    hostos: {

      packages: {

        hostos-boot-release: {

          forcereboot: yes

          flags: ["-n dal3-qz2-sr3-rk278-s08 --secureboot enable --enable-tdx"]

          artifrepo: [docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local](http://docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local)

          tag: 7.0.0-20240613T135451Z_25ea334

        }

        hostos-config-release: {

          artifrepo: [docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local](http://docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local)

          flags: ["-n dal3-qz2-sr3-rk278-s08"]

          tag: 7.0.0-20240613T151302Z_2a90e83

        }

        hostos-base-os-sw-release: {

          artifrepo: [docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local](http://docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local)

          flags: ["-n dal3-qz2-sr3-rk278-s08"]

          tag: 7.0.0-20240611T174457Z_c741ed7

        }

        hostos-base-net-sw-release: {

          artifrepo: [docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local](http://docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local)

          flags: ["-n dal3-qz2-sr3-rk278-s08"]

          tag: 7.0.0-20240511T115328Z_e0723dd

        }

        hostos-nextgen-os-sw-release:{

          artifrepo: [docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local](http://docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local)

          flags: ["-n dal3-qz2-sr3-rk278-s08"]

          tag: 7.0.0-20240612T155322Z_e2ddc8c

        }

        hostos-post-config-release: {

          artifrepo: [docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local](http://docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local)

          flags: ["-n dal3-qz2-sr3-rk278-s08"]

          tag: 7.0.0-20240613T151302Z_2a90e83

        }

      }

    }

  }

  mzone: mzone7304

}
```

## Jira

- Epic :

[CIO-10495](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10495)
