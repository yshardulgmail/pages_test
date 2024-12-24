---
layout: default
title: Pipelines 
parent: Secure Supply chain 
grand_parent: Hypervisor Hardening
---



## Supply Chain pipelines


This document lists all the automated supply chain pipelines that are used to securely migrate HostOS artifacts and associated pipeline dependencies

Pipeline alerts are published in the slack channel at: <https://ibmcloudlab.slack.com/archives/C03V3NQ5G95>

## Artifact Migration Pipelines



| Pipeline | Link | Description | Source code repository |
| --- | --- | --- | --- |
| Canonical Boot image Migration | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/SecureSupplyChain/job/hostos\_cos\_image\_migration/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/SecureSupplyChain/job/hostos_cos_image_migration/) | Migrate boot images from Canonical Cloud Object Storage Bucket to IBM Jfrog ArtifactoryImages are published to the following Jfrog artifactory repositories**wcp\-canonical\-cos\-team\-generic\-local**:  production registry used by CI pipelines for hostos\-boot\-payload builds**wcp\-genctl\-sandbox\-generic\-local**: sandbox development registry used by developers for custom hostos\-boot\-payload builds | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/cos\-artifactory\-migration](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/cos-artifactory-migration) |
| Canonical Kernel Patch Migration | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/SecureSupplyChain/job/hostos\_cos\_image\_migration/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/SecureSupplyChain/job/hostos_cos_image_migration/) | Migrate kernel patches from Canonical Cloud Object Storage Bucket to IBM Jfrog ArtifactoryPatches are published to the following Jfrog artifactory repositories**wcp\-hostos\-production\-team\-generic\-local**:  production registry used by CI pipelines for hostos\-kernel\-patch\-payload builds**wcp\-genctl\-sandbox\-generic\-local**: sandbox development registry used by developers for custom hostos\-kernel\-patch\-payload builds | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/cos\-artifactory\-migration](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/cos-artifactory-migration) |
| Canonical PPA Debian Migration | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/SecureSupplyChain/job/hostos\_repository\_mirror\_sync/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/SecureSupplyChain/job/hostos_repository_mirror_sync/) | Migrates debian packages from following Canonical PPA repos to IBM Jfrog artifactory reposPackages are scanned and picked for Bionic and Jammy Ubuntu Releases**[Release PPA repo](https://launchpad.net/~ibm-cloud/+archive/ubuntu/release)** → **wcp\-hostos\-production\-team\-release\-ppa\-debian\-local** (Jfrog)Release PPA provides the finalised debian packages that are published by Canonical for IBM use. These mainly include packages such as qemu, libvirt, kerberos, rclone and many others**[Proposed PPA repo](https://launchpad.net/~ibm-cloud/+archive/ubuntu/proposed)** → **wcp\-hostos\-production\-team\-proposed\-ppa\-debian\-local** (Jfrog)Proposed PPA provides experimental debian packages that are yet to be assessed for IBM use. These mainly include packages such as qemu, libvirt, kerberos, rclone and many others | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/canonical\-ppa\-repo\-sync](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/canonical-ppa-repo-sync) |
| Intel SGX Repository Sync | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/SecureSupplyChain/job/hostos\_repository\_mirror\_sync/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/SecureSupplyChain/job/hostos_repository_mirror_sync/) | Migrates third party debian packages from  [Intel SGX](https://download.01.org/intel-sgx/sgx_repo/ubuntu) repository to HostOS Artifactory repositoryArtifactory repository: **wcp\-hostos\-production\-team\-third\-party\-debian\-local** | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/canonical\-ppa\-repo\-sync](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/canonical-ppa-repo-sync) |
| Ubuntu Pro Repository sync | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/SecureSupplyChain/job/hostos\_repository\_mirror\_sync/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/SecureSupplyChain/job/hostos_repository_mirror_sync/) | Mirrors debian packages from Ubuntu Pro ESM and FIPS repositories to IBM Artifactory Jfrog debian repository at **wcp\-hostos\-production\-team\-ubuntu\-pro\-debian\-local** | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/canonical\-ppa\-repo\-sync](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/canonical-ppa-repo-sync) |
| Third Party Debian. Migration | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/SecureSupplyChain/job/hostos\_migrate\_third\_party\_packages/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/SecureSupplyChain/job/hostos_migrate_third_party_packages/) | Migrates third party packages such as Nessus to IBM Artifactory repositoryNessus Repository: [https://www.tenable.com/downloads/api/v2/pages/nessus\-agents](https://www.tenable.com/downloads/api/v2/pages/nessus-agents)IBM Artifactory repository: **wcp\-hostos\-production\-team\-third\-party\-debian\-local** | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/third\-party\-package\-migration](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/third-party-package-migration) |

## Inventory and maintenance automation pipelines



| Pipeline | Link | Description | Source code repository |
| --- | --- | --- | --- |
| Manage CI Build agents | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/InventoryAutomation/job/manage\_ci\_build\_agents/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/InventoryAutomation/job/manage_ci_build_agents/) | This build pipeline helps to maintain and recycle a required pool of fyre based build agents used for many HostOS utility buildsBuild agents are recycled to ensure that the latest OS patches and updates are installed to maintain compliance | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/jenkins\-build\-agent\-mgmt](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/jenkins-build-agent-mgmt) |
| Functional Id password reset reminder | [https://wcp\-genctl\-nextgen\-team\-hostos\-jenkins.swg\-devops.com/view/InventoryAutomation/job/fid\_password\_expiry\_reminder/](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/view/InventoryAutomation/job/fid_password_expiry_reminder/) | This build utility helps to track and alert Functional Id admins to reset account passwords on a periodic basisThe build maintains fid password reset dates as secrets in the Jenkins config at :[hostosprd fid](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/credentials/store/system/domain/_/credential/HOSTOS_CI_HSTOSPRD_PWD_RST_DT)[clhostos fid](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/credentials/store/system/domain/_/credential/HOSTOS_CI_CLHOSTOS_PWD_RST_DT)[hostostest fid](https://wcp-genctl-nextgen-team-hostos-jenkins.swg-devops.com/credentials/store/system/domain/_/credential/HOSTOS_CI_HOSTOSTEST_PWD_RST_DT)The date format  YYYY\-MM\-DD | [https://github.ibm.com/cloudlab/hostos\-misc\-tools/tree/master/utils/fid\-password\-reset\-reminder](https://github.ibm.com/cloudlab/hostos-misc-tools/tree/master/utils/fid-password-reset-reminder) |



