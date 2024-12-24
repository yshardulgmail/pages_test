---
layout: default
title: Process Authentication Group (PAG) login
parent: NGDC
grand_parent: Developer Information
---

## Process Authentication Group (PAG) login

 ## Prerequisites

As per the [Usage Dependencies](https://pages.github.ibm.com/bedrock-squad/ng-fabric-docs/bastion-solution-onboarding-ngdc-w3id.html) for PAG you must first;

* Install Global Protect, and be connected to the Global Protect VPN when trying to use PAG.
* Download and install the [PAG cli](https://github.ibm.com/underlay-fabric-bastion-cli/cli-release/releases) and follow below steps to make it executable - 
```
sudo cp $HOME/Downloads/pag /usr/local/bin
sudo chmod +x /usr/local/bin/pag
sudo xattr -dr com.apple.quarantine /usr/local/bin/pag
```
* Register your yubikey (series 5 or above) with [w3id](https://login.w3.ibm.com/usc/settings/security)

## AccessHub groups

In [AccessHub](https://ibm-support.saviyntcloud.com/ECMv6/request/requestHome), you must request the following groups;

* IaaS Access Management \-\> fabng\-feddev\-ucnat
* NGDC Bastion Dev \-\> Fabric Writer
* Nextgen NS3 User Management \-\> ngdc\-ns3\-jump\-preprod\-operator

## ServiceNow \-

* The validation of the Service Now incident/problem task/case/change request ticket is performed by leveraging an OSS API layer and is based on the following criteria:
	+ The ticket must be active (i.e. open status), and approved (approval applies to Change Requests only)
	+ the ticket must be either assigned to the operator (for Change Requests) or assigned to the group the operator belongs to (for incidents/problem tasks and cases).The assignment of a Service Now ticket can be either implemented using the Assignment Groups or using the Assistant Groups. Assistant Groups can be added to a Service Now ticket from whomever has visibility of the ServiceNow ticket.
* For non\-production environment use pre\-approved change template in Service Now [ANY TRIBE \- Pre\-production environment testing](https://watson.service-now.com/change_request.do?sys_id=-1&sysparm_catalog=e0d08b13c3330100c8b837659bba8fb4&sysparm_catalog_view=catalog_default&sysparm_link_parent=b0fdfb01932002009ca87a75e57ffbe9&sysparm_query=type%3dstandard%5estd_change_producer_version%3ddd1fd0e0970d7910bdc850900153afa8&sysparm_view=Default+view&sysparm_view_forced=true).
* sample snow ticket:[CHG8927255](https://urldefense.proofpoint.com/v2/url?u=https-3A__watson.service-2Dnow.com_nav-5Fto.do-3Furi-3Dchange-5Frequest.do-253Fsys-5Fid-3Db17551a1471452509d7cf46c416d431e-2526sysparm-5Fstack-3Dchange-5Frequest-5Flist.do-253Fsysparm-5Fquery-3Dactive-3Dtrue&d=DwMFaQ&c=BSDicqBQBDjDI9RkVyTcHQ&r=y_ew8WB1uAuMmomwR2jc96TdCRw4wE35nNG03kM9zYc&m=FH21LJkMIYopWhc-C7cR4ZuPf4hqYWEdRIM0YZuFo6CU5hxA-MfLWfBAg_U1BGxv&s=vCt0AunBjtsUT196wMp5cy0SuCjkUZKxXzY3szaFXxw&e=). Please assign the ticket to yourself and select the `Configuration item` that belongs to your team. (**is\-hostos**)

## Login to PAG

The example below shows logging in to the qz4/feddev jump host. At time of writing this is the only environment available via pag, but you may need to adapt the example choices given below for future environments as they are onboarded.

From a terminal;

* `pag login`
	+ Select `ngdc` for IaaS env
	+ Select `development` for target env
	+ Select `feddev` for target deployment


```
shashikantshelke@Shashikants-MacBook-Pro Downloads % ./pag-3 login  
2024/08/27 22:12:20 Fetching target enviroments ...   
Select an IaaS env:  
1. ngdc  
2. classic  
Enter a number> 1  
Select a target env:  
1. development  
2. staging  
3. prod  
Enter a number> 1  
Select a target deployment:  
1. feddev  
2. ngdc-int  
Enter a number> 1  
2024/08/27 22:12:32 Opening w3id OIDC auth endpoint in default browser  
2024/08/27 22:12:32 If browser window does not open automatically, open it by clicking on the link:   
<https://login.w3.ibm.com/oidc/endpoint/default/authorize?client_id=MDJjMDU4OGEtZDdjNy00&redirect_uri=https%3A%2F%2Flocalhost%3A8888%2Fpag%2Flogin&response_type=code&scope=openid>  

./pag ssh --connect 198.19.0.5 --ticketing-system snow --ticket-id <>
```  

Note: select the Passkey option instead of w3id Credentials (as w3 is moving towards password-less login).

Register your Yubikey 5 series and above with [w3id](https://login.w3.ibm.com/usc/settings/security) for MFA (use PassKey option).

## SCP using PAG 

* Ensure PAG CLI version is 1.0.7 or higher 
```
shashikantshelke@MacBookPro ~ % ./pag version                                             
CLI version: 1.0.7
```

* Download PAG certificates 
* Use PAG cli to download the certificate/key needed for perform scp. 
```
$ ./pag login --iaas-env classic --target-env prod --target-deployment dal10

$ ./pag ssh cert
PAG cert/key downloaded successfully
 Cert path : <user_home_dir>/.pag/dal10-api-user-cert.pub
 Key path : <user_home_dir>/.pag/dal10-api-user
```
* Verify if the added certificate is valid
```
$ ssh-add -l
```
* Configure ssh_config on your local system with host entries. You can have multiple entries as shown below.
	+ IdentityFile will be the Key Path returned as part of pag ssh cert command
	+ CertificateFile will be the Cert Path returned as part of pag ssh cert command
	+ Hostname to be fetched from <user_home_dir>/.pag/config. Its the value of bastion_ssh_endpoint key in this pag config.
	+ User format is <sl_user_name>@<targetted_jumphost>:22
```
shashikantshelke@MacBookPro ~ % cat ~/.ssh/config
Host dal10scp
  User shshelke@shellngqztwodal1001:22
  Hostname ssh-service-fcp-prod-dal10.bastion.k8s-dal10.softlayer.local
  Port 3023
  ForwardAgent true
  IdentitiesOnly yes
  IdentityFile /Users/shashikantshelke/.pag/dal10-api-user
  CertificateFile /Users/shashikantshelke/.pag/dal10-api-user-cert.pub
```
* Use scp command to copy to/from from the desired jumphost. Use the Host alias as created in above ssh config.
	+ Download file from jumphost:
	```
	$ scp -o ForwardAgent=yes <Host>:<user_home_dir>/<file_name> <file_name>
	```
	+ Upload file to jumphost:
	```
	$ scp -o ForwardAgent=yes <file_name> <Host>:<user_home_dir>/<file_name> 
	```
* Certificate/Key remain valid for 1 hour after login with PAG CLI.
* As the local ssh-agent is being forwarded to PAG, for every new login to a different data center, the previous PAG ssh key details is 
being removed from ssh-agent. In case the user wants to continue doing the scp to the previously configured host in ssh config, they 
will have to re-add the ssh key to the local ssh-agent before performing the scp.
  ```
  $ ssh-add <user_home_dir>/.pag/dal10-api-user
  ```
  
* In case the ssh-agent is having too many PAG related ssh keys, please delete the keys that are not required, as having too many keys 
from different PAG data center can cause connection failure due to too many retries. And re-add the required ssh key to the local ssh-agent before performing the scp. 
  
```
  $ ssh-add -D
  $ ssh-add <user_home_dir>/.pag/dal10-api-user
``` 

