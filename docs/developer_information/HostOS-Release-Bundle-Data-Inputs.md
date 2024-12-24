---
layout: default
title: Release Bundle Input 
parent: Developer Information
---

## HostOS Release Bundle Data Inputs


This is to document the input data that is required by every HostOS release bundle and the output return codes for
 the deployment service.


  



As we are aware, every HostOS release bundle is a docker image and the runtime inputs to the image container are
 supplied as follows:


1. Deployment initiation command as container entrypoint override. Typically we use command such as
 follows:  
nextgen\-hostos\-boot → hostos\-boot\-release  
nextgen\-hostos\-config →
 hostos\-config\-release  
nextgen\-hostos\-upgrade → hostos\-base\-os\-sw\-release, hostos\-base\-net\-sw\-release,
 hostos\-nextgen\-os\-sw\-release
2. Deployment initiation command arguments. These are passed after the entrypoint command override.  
Example:
 nextgen\-hostos\-boot \-\-force\-reboot
3. Configuration files and folders are attached as volumes to the container
4. Environment variables passed to the container


  



Please find below a comprehensive list of all input data supported by HostOS release bundles


#### Release Bundle: hostsos\-boot\-release


#### Input Command: nextgen\-hostos\-boot




| **Argument** | **Type** | **Description** | **Required** | **env var** | **Description** | **input volumes (target location)** | **Volume description** | **Output return codes** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| \-\-force\-reboot | Boolean | forces nodes to reboot that are already at the desired level | No | DNS\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L24](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L24) | /var/lib/nextgen/vault | mounts vault certs and vault secrets files. Example:  bmc\-passwordbmc\-usernamecertshost\-passwordhost\-usernamessh\-private\-keyssh\-private\-key.decryssh\-private\-key.encryssh\-private\-key.newtrusted\-user\-ca.pub | 1 \-\> Task Failed0 \-\> All Success\-1 \-\> Skipped |
| \-\-simpleboot | Boolean | Boot s390x nodes with Simple Booter.Applicable only for 5\.x release version | yes (Only for 5\.x s390x deployments) | DOCKER\_REG | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L31](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L31) | /var/lib/nextgen/inventory/undercloud | mounts the undercloud directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/undercloud](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/undercloud) |  |
| \-\-secureboot ('enable', 'disable') | Choice | Enable/disable SecureBoot mode in BIOS . Applicable only for 6\.x release version | No | NTP\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L41](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L41) | /tmp/.ngsec | mounts the time sensitve vault token created |  |
| \-\-kernelparam | String | kernel boot CLI ParameterExample : "default\_hugepagesz\=1G  hugepagesz\=1G"Applicable only for 6\.x release version | No | VAULT\_TOKEN | Time sensitive vault token | /var/lib/nextgen/inventory/mzonexxxx.yml | mounts the platform\-inventory file for the mzone under deployment |  |
| \-\-keys | Boolean | Enroll Secure Boot public keys to MOK | No | VAULT\_CACERT | location of vault certificate in the mounted volume at /var/lib/nextgen/vaultexample:  /var/lib/nextgen/vault/certs/vault\-dal\-intermediary\-ca.crt | /var/log/nextgen | mounts host directory for storing deployemnt logs |  |
| \-\-inv\-file \<filepath\> | String | specifies the yaml file containing the m\-zone inventory | No |  |  | /tmp/misc | mounts the region directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/) |  |
| \-\-release\-manifest \<filepath\> | String | specifies the yaml file containing the release manifest | No |  |  |  |  |  |
| \-\-vault\-info\-dir \<dirpath\> | String | specifies the directory containing the vault access info | No |  |  |  |  |  |
| \-\-node\-list \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to include | No |  |  |  |  |  |
| \-\-exclude\-nodes \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to exclude | No |  |  |  |  |  |
| \-\-verify\-only | Boolean | only does the verification and doesn't run the operation | No |  |  |  |  |  |
| \-\-disruption\-level \<level\> | String | specifies the maximum level of disruption that is allowe | No |  |  |  |  |  |
| \-\-logging\-dir \<dirpath\> | String | specifies the directory to log output from the command | No |  |  |  |  |  |
| \-\-logging\-level \<level\> | String | specifies operation logs verbosity | No |  |  |  |  |  |
| \-\-assume\-yes | Boolean | suppresses the prompts to verify running the operation | No |  |  |  |  |  |
| \-\-wait\-on\-ssh "integer" | Integer | Specifies how many seconds to wait for SSH availability on the system | No |  |  |  |  |  |
| \-\-override\-vers\-checking | Boolean | Specifies whether to override hostos version checking | No |  |  |  |  |  |
| \-\-is\-virtual\-environment | Boolean | Specifies whether it runs on virtual environment | No |  |  |  |  |  |
| \-\-max\-concurrent "integer" | Integer | Throw in an internal parameter to specify the maximum concurrent threads | No |  |  |  |  |  |
| \-\-no\-cleanup | Boolean | Throw in an internal debug flag to not do cleanup of payloads from nodes | No |  |  |  |  |  |


#### Release Bundle: hostos\-config\-release


#### Input Command: nextgen\-hostos\-config




| **Argument** | **Type** | **Description** | **Required** | **env var** | **Description** | **input volumes (target location)** | **Volume description** | **Output return codes** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| \-\-expand\-only | Boolean | limit the config operation to expand only activities | No | DNS\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L24](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L24) | /var/lib/nextgen/vault | mounts vault certs and vault secrets files. Example:  bmc\-passwordbmc\-usernamecertshost\-passwordhost\-usernamessh\-private\-keyssh\-private\-key.decryssh\-private\-key.encryssh\-private\-key.newtrusted\-user\-ca.pub | 1 \-\> Task Failed0 \-\> All Success\-1 \-\> Skipped |
| \-\-command\-timeout \<integer\> | Integer | wait time for commands to timeout (default 1800 sec) | No | DOCKER\_REG | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L31](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L31) | /var/lib/nextgen/inventory/undercloud | mounts the undercloud directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/undercloud](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/undercloud) |  |
| \-\-opal\-rotate\-passphrase | Boolean | rotate passphrases for the opal drives | No | NTP\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L41](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L41) | /tmp/.ngsec | mounts the time sensitve vault token created |  |
| \-\-inv\-file \<filepath\> | String | specifies the yaml file containing the m\-zone inventory | No | VAULT\_TOKEN | Time sensitive vault token | /var/lib/nextgen/inventory/mzonexxxx.yml | mounts the platform\-inventory file for the mzone under deployment |  |
| \-\-release\-manifest \<filepath\> | String | specifies the yaml file containing the release manifest | No | VAULT\_CACERT | location of vault certificate in the mounted volume at /var/lib/nextgen/vaultexample:  /var/lib/nextgen/vault/certs/vault\-dal\-intermediary\-ca.crt | /var/log/nextgen | mounts host directory for storing deployemnt logs |  |
| \-\-vault\-info\-dir \<dirpath\> | String | specifies the directory containing the vault access info | No |  |  | /tmp/misc | mounts the region directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/) |  |
| \-\-node\-list \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to include | No |  |  |  |  |  |
| \-\-exclude\-nodes \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to exclude | No |  |  |  |  |  |
| \-\-verify\-only | Boolean | only does the verification and doesn't run the operation | No |  |  |  |  |  |
| \-\-disruption\-level \<level\> | String | specifies the maximum level of disruption that is allowe | No |  |  |  |  |  |
| \-\-logging\-dir \<dirpath\> | String | specifies the directory to log output from the command | No |  |  |  |  |  |
| \-\-logging\-level \<level\> | String | specifies operation logs verbosity | No |  |  |  |  |  |
| \-\-assume\-yes | Boolean | suppresses the prompts to verify running the operation | No |  |  |  |  |  |
| \-\-wait\-on\-ssh "integer" | Integer | Specifies how many seconds to wait for SSH availability on the system | No |  |  |  |  |  |
| \-\-override\-vers\-checking | Boolean | Specifies whether to override hostos version checking | No |  |  |  |  |  |
| \-\-is\-virtual\-environment | Boolean | Specifies whether it runs on virtual environment | No |  |  |  |  |  |
| \-\-max\-concurrent "integer" | Integer | Throw in an internal parameter to specify the maximum concurrent threads | No |  |  |  |  |  |
| \-\-no\-cleanup | Boolean | Throw in an internal debug flag to not do cleanup of payloads from nodes | No |  |  |  |  |  |


#### Release Bundle: hostos\-base\-os\-sw\-release, hostos\-base\-net\-sw\-release, hostos\-nextgen\-os\-sw\-release


#### Input Command: nextgen\-hostos\-upgrade




| **Argument** | **Type** | **Description** | **Required** | **env var** | **Description** | **input volumes (target location)** | **Volume description** | **Output return codes** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| \-\-inv\-file \<filepath\> | String | specifies the yaml file containing the m\-zone inventory | No | DNS\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L24](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L24) | /var/lib/nextgen/vault | mounts vault certs and vault secrets files. Example:  bmc\-passwordbmc\-usernamecertshost\-passwordhost\-usernamessh\-private\-keyssh\-private\-key.decryssh\-private\-key.encryssh\-private\-key.newtrusted\-user\-ca.pub | 1 \-\> Task Failed0 \-\> All Success\-1 \-\> Skipped |
| \-\-release\-manifest \<filepath\> | String | specifies the yaml file containing the release manifest | No | DOCKER\_REG | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L31](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L31) | /var/lib/nextgen/inventory/undercloud | mounts the undercloud directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/undercloud](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/undercloud) |  |
| \-\-vault\-info\-dir \<dirpath\> | String | specifies the directory containing the vault access info | No | NTP\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L41](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L41) | /tmp/.ngsec | mounts the time sensitve vault token created |  |
| \-\-node\-list \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to include | No | VAULT\_TOKEN | Time sensitive vault token | /var/lib/nextgen/inventory/mzonexxxx.yml | mounts the platform\-inventory file for the mzone under deployment |  |
| \-\-exclude\-nodes \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to exclude | No | VAULT\_CACERT | location of vault certificate in the mounted volume at /var/lib/nextgen/vaultexample:  /var/lib/nextgen/vault/certs/vault\-dal\-intermediary\-ca.crt | /var/log/nextgen | mounts host directory for storing deployemnt logs |  |
| \-\-verify\-only | Boolean | only does the verification and doesn't run the operation | No |  |  | /tmp/misc | mounts the region directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/) |  |
| \-\-disruption\-level \<level\> | String | specifies the maximum level of disruption that is allowe | No |  |  |  |  |  |
| \-\-logging\-dir \<dirpath\> | String | specifies the directory to log output from the command | No |  |  |  |  |  |
| \-\-logging\-level \<level\> | String | specifies operation logs verbosity | No |  |  |  |  |  |
| \-\-assume\-yes | Boolean | suppresses the prompts to verify running the operation | No |  |  |  |  |  |
| \-\-wait\-on\-ssh "integer" | Integer | Specifies how many seconds to wait for SSH availability on the system | No |  |  |  |  |  |
| \-\-override\-vers\-checking | Boolean | Specifies whether to override hostos version checking | No |  |  |  |  |  |
| \-\-is\-virtual\-environment | Boolean | Specifies whether it runs on virtual environment | No |  |  |  |  |  |
| \-\-max\-concurrent "integer" | Integer | Throw in an internal parameter to specify the maximum concurrent threads | No |  |  |  |  |  |
| \-\-no\-cleanup | Boolean | Throw in an internal debug flag to not do cleanup of payloads from nodes | No |  |  |  |  |  |


#### Release Bundle: hostos\-kernel\-patch\-release


#### Input Command: nextgen\-hostos\-kernel\-patch




| **Argument** | **Type** | **Description** | **Required** | **env var** | **Description** | **input volumes (target location)** | **Volume description** | **Output return codes** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| \-\-inv\-file \<filepath\> | String | specifies the yaml file containing the m\-zone inventory | No | DNS\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L24](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L24) | /var/lib/nextgen/vault | mounts vault certs and vault secrets files. Example:  bmc\-passwordbmc\-usernamecertshost\-passwordhost\-usernamessh\-private\-keyssh\-private\-key.decryssh\-private\-key.encryssh\-private\-key.newtrusted\-user\-ca.pub | 1 \-\> Task Failed0 \-\> All Success\-1 \-\> Skipped |
| \-\-release\-manifest \<filepath\> | String | specifies the yaml file containing the release manifest | No | DOCKER\_REG | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L31](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L31) | /var/lib/nextgen/inventory/undercloud | mounts the undercloud directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/undercloud](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/undercloud) |  |
| \-\-vault\-info\-dir \<dirpath\> | String | specifies the directory containing the vault access info | No | NTP\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L41](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L41) | /tmp/.ngsec | mounts the time sensitve vault token created |  |
| \-\-node\-list \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to include | No | VAULT\_TOKEN | Time sensitive vault token | /var/lib/nextgen/inventory/mzonexxxx.yml | mounts the platform\-inventory file for the mzone under deployment |  |
| \-\-exclude\-nodes \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to exclude | No | VAULT\_CACERT | location of vault certificate in the mounted volume at /var/lib/nextgen/vaultexample:  /var/lib/nextgen/vault/certs/vault\-dal\-intermediary\-ca.crt | /var/log/nextgen | mounts host directory for storing deployemnt logs |  |
| \-\-verify\-only | Boolean | only does the verification and doesn't run the operation | No |  |  | /tmp/misc | mounts the region directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/) |  |
| \-\-disruption\-level \<level\> | String | specifies the maximum level of disruption that is allowe | No |  |  |  |  |  |
| \-\-logging\-dir \<dirpath\> | String | specifies the directory to log output from the command | No |  |  |  |  |  |
| \-\-logging\-level \<level\> | String | specifies operation logs verbosity | No |  |  |  |  |  |
| \-\-assume\-yes | Boolean | suppresses the prompts to verify running the operation | No |  |  |  |  |  |
| \-\-wait\-on\-ssh "integer" | Integer | Specifies how many seconds to wait for SSH availability on the system | No |  |  |  |  |  |
| \-\-override\-vers\-checking | Boolean | Specifies whether to override hostos version checking | No |  |  |  |  |  |
| \-\-is\-virtual\-environment | Boolean | Specifies whether it runs on virtual environment | No |  |  |  |  |  |
| \-\-max\-concurrent "integer" | Integer | Throw in an internal parameter to specify the maximum concurrent threads | No |  |  |  |  |  |
| \-\-no\-cleanup | Boolean | Throw in an internal debug flag to not do cleanup of payloads from nodes | No |  |  |  |  |  |


#### Release Bundle: hostos\-validate\-release


#### Input Command: nextgen\-hostos\-validate




| **Argument** | **Type** | **Description** | **Required** | **env var** | **Description** | **input volumes (target location)** | **Volume description** | **Output return codes** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| \-\-inv\-file \<filepath\> | String | specifies the yaml file containing the m\-zone inventory | No | DNS\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L24](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L24) | /var/lib/nextgen/vault | mounts vault certs and vault secrets files. Example:  bmc\-passwordbmc\-usernamecertshost\-passwordhost\-usernamessh\-private\-keyssh\-private\-key.decryssh\-private\-key.encryssh\-private\-key.newtrusted\-user\-ca.pub | 1 \-\> Task Failed0 \-\> All Success\-1 \-\> Skipped |
| \-\-release\-manifest \<filepath\> | String | specifies the yaml file containing the release manifest | No | DOCKER\_REG | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L31](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L31) | /var/lib/nextgen/inventory/undercloud | mounts the undercloud directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/undercloud](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/undercloud) |  |
| \-\-vault\-info\-dir \<dirpath\> | String | specifies the directory containing the vault access info | No | NTP\_SERVER | Obtained from mzone undercloud file. Example [https://github.ibm.com/cloudlab/platform\-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2\-qz2\-undercloud.yml\#L41](https://github.ibm.com/cloudlab/platform-inventory/blob/4638b8bc736de65c0b1e2f37bdb4d7ab756d1370/region/undercloud/dal2-qz2-undercloud.yml#L41) | /tmp/.ngsec | mounts the time sensitve vault token created |  |
| \-\-node\-list \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to include | No | VAULT\_TOKEN | Time sensitive vault token | /var/lib/nextgen/inventory/mzonexxxx.yml | mounts the platform\-inventory file for the mzone under deployment |  |
| \-\-exclude\-nodes \<nodelist\> | Comma seperated list | specifies the comma\-separated list/file of nodes to exclude | No | VAULT\_CACERT | location of vault certificate in the mounted volume at /var/lib/nextgen/vaultexample:  /var/lib/nextgen/vault/certs/vault\-dal\-intermediary\-ca.crt | /var/log/nextgen | mounts host directory for storing deployemnt logs |  |
| \-\-verify\-only | Boolean | only does the verification and doesn't run the operation | No |  |  | /tmp/misc | mounts the region directory of paltform\-inventory at [https://github.ibm.com/cloudlab/platform\-inventory/tree/master/region/](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region/) |  |
| \-\-disruption\-level \<level\> | String | specifies the maximum level of disruption that is allowe | No |  |  |  |  |  |
| \-\-logging\-dir \<dirpath\> | String | specifies the directory to log output from the command | No |  |  |  |  |  |
| \-\-logging\-level \<level\> | String | specifies operation logs verbosity | No |  |  |  |  |  |
| \-\-assume\-yes | Boolean | suppresses the prompts to verify running the operation | No |  |  |  |  |  |
| \-\-wait\-on\-ssh "integer" | Integer | Specifies how many seconds to wait for SSH availability on the system | No |  |  |  |  |  |
| \-\-override\-vers\-checking | Boolean | Specifies whether to override hostos version checking | No |  |  |  |  |  |
| \-\-is\-virtual\-environment | Boolean | Specifies whether it runs on virtual environment | No |  |  |  |  |  |
| \-\-max\-concurrent "integer" | Integer | Throw in an internal parameter to specify the maximum concurrent threads | No |  |  |  |  |  |
| \-\-no\-cleanup | Boolean | Throw in an internal debug flag to not do cleanup of payloads from nodes | No |  |  |  |  |  |


  



Following are the attribute fields used by hostos release bundles from the
 platform\-inventory manifests




| Field/ Attribute | Attribute Description | Attribute Type | Source | Usage |
| --- | --- | --- | --- | --- |
| nodes | Describes a particular system or node in the m\-zone | array | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| master\_node | Describes a master node in the m\-zone | array | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| control\_node | Describes a control node in the m\-zone | array | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| compute\_node | Describes a compute node in the m\-zone | array | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| acadia\_node | Describes a acadia node in the m\-zone | array | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| mgmt\_node | Describes a management node in the m\-zone | array | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| environment\_type | Whether the environment is development, staging or production | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| undercloud | The name of the datacenter or undercloud this m\-zone is part of. | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| hostname | The hostname of the host operating system of the node. | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| hostIP | The ip address of the host operating system of the node | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| fabricIP | The ip address of the system in the fabric | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| FPextIP | The FP external ip address of the system in the m\-zone | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| MAC | The mac address of the system in the m\-zone | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| FPMAC | The FP Mac address of the system in the m\-zone | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| uuid | The hardware uuid of the system in the m\-zone | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| ipmi | The ip address to access the system's BMC thru IPMI | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| edgeIP | The ip address of the edge node in the m\-zone | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| arch | The architecture of the host operating system of the node | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| master | Whether or not the given node is a master node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| control | Whether or not the given node is a control node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| compute | Whether or not the given node is a compute node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| edge | Whether or not the given node is an edge node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| nfs | Whether or not the given node is a dev NFS Master | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| acadia | Whether or not the given node is a acadia node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| mgmt | Whether or not the given node is a mgmt node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| baremetalproxy | Whether or not the given node is a baremetalproxy node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| gpu | Whether or not the given compute node has gpus | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| localdisk | Whether or not the given compute node has local disks | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| baremetal | Whether or not the given compute node is a bare metal node | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| smartnic | The specific type of smartnic used by this node, if any | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| profileClass | The VSI profile class for this node | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| uefi | Whether or not the given node is in uefi mode | boolean | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| externalnetworks | Describes the configuration of the external network for this node | array | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| [externalnetworks.name](http://externalnetworks.name) | The name given to the network described | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| externalnetworks.description | The description of the network described | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| externalnetworks.subnet | The subnet mask of the network in CIDR notation | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| externalnetworks.dns | The list of ip addresses of the DNS servers for the network | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| externalnetworks.type | Whether this network is public or private | string | mzone.yml from platform\-inventory | All HostOS Release Bundles |
| services.dns.addresses | The list of DNS servers for the given undercloud | array | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.docker\_registry.addresses | The list of Docker registries for the given undercloud | array | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.ntp.addresses | The list of NTP servers for the given undercloud | array | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.vault.management\_ip | The management ip for vault for the given undercloud | string | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.vault.intermediary\_cert | The intermediary cacert for vault for the given undercloud | string | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.vault.namespace | The namespace for vault for the given undercloud | string | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.vault.prefix | The prefix for vault for the given undercloud | string | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| [services.datacenter.name](http://services.datacenter.name) | The name of the datacenter for the given undercloud | string | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.datacenter.region | The region of the datacenter for the given undercloud | string | undercloud.yml from platform\-inventory | All HostOS Release Bundles |
| services.datacenter.index | The datacenter index in a given region , ex DAL10, DAL12 for DALLAS region | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.iam.url | The iam url for the given undercloud | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.fim\_vpn.vpn\_vm\_ip | VPN vm ip for FIM(file integrity) | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.fim\_vpn.server\_url | VPN server url for FIM(file integrity) | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.fim\_vpn.server\_cidr | VPN server cidr range for FIM(file integrity) | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.fim\_vpn.fim\_user | user account FIM(file integrity) | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.falcon\_sensor.CID | Crowdstrike falcon sensor attribute | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.qradar\_endpoint.addresses | Qradar endpoint arraylist | array | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.qradar\_endpoint.tls | Qradar endpoint tls protocol | boolean | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.bes\_relay.addresses | BES relay server addresses | array | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.RRs.ASN | Route reflector ASN | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.RRs.addresses | Route reflector servers | array | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.iptables.bypasschecksum | Flag used to update the HOSTOS IP Tables rules to the new set of rules if they changed | boolean | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.iptables.bypassrestorecheck | Flag used to update the HOSTOS IP Tables rules to the new set of rules if they changed | boolean | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.livemigration.bypass\_agent | VPC vm livemigration settings | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.livemigration.type | VPC vm livemigration settings | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.zabbix\_endpoint.addresses | Zabbix endpoint address | array | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.nessus.cse.proxy\_host | Nessus scanner proxy host | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.nessus.addresses | Nessus scanner service address | array | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.cos\_flowlogs.flowlogs\_size | Flow logs size atribute | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.cos\_flowlogs.endpoint\_address | Flow logs cloud object storage endpoint address | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| physical\_servers.xcatnet\_ip\_range | Deploy server ip addresses | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.kdump.remote\_path | Kdump config | string | undercloud.yml from platform\-inventory | hostos\-config\-release only |
| services.kdump.addresses | Kdump config | array | undercloud.yml from platform\-inventory | hostos\-config\-release only |


  



## HostOS Node personalities


Personalities indicates the role a given node in an mzone is supposed to
 perform.  
  
We have the following primary personalities:


* master
* control
* compute


And secondary personalities which can co\-exist in combination with each other and any one of the above primary
 personalities


* edge
* nfs
* gpu
* localdisk
* service
* acadia
* mgmt


For example we can have an mzone node designated as Compute and any of the
 secondary personalities such as service, nfs etc


  





| **HostOS Personalities** | **Description** |
| --- | --- |
| master | Mzone master node |
| control | Mzone control node |
| compute | Mzone compute node |
| edge | Mzone node designated as edge |
| nfs | Mzone node designated as nfs |
| gpu | Mzone node with gpu hardware |
| localdisk | Mzone node with localdisk storage |
| service | Mzone node designated as service |
| acadia | Mzone node for acadia services |
| mgmt | Mzone node for management services |


  


