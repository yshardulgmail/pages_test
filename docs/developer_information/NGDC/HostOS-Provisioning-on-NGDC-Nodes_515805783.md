---
layout: default
title: Provisioning 
parent: NGDC
grand_parent: Developer Information
---
##  HostOS Provisioning on NGDC Nodes

VPC Nodes designated for HostOS use for ctrl and compute node types (no mgmt). These nodes are in the VPC (not Fabric group).

\- dal12, VPC group \- HostOS roles (compute, control,etc) for qz4 rack45:   
      \- s02: 192\.168\.45\.4 (dal09\-qz4\-sr07\-rk045\-s02\)   
      \- s04: 192\.168\.45\.8 (dal09\-qz4\-sr07\-rk045\-s04\)  
  
Use netbox config context: vpc\_zone\_ctrl for provisioning.  
  
\- 4 additional [hardware in rack23](https://confluence.swg.usma.ibm.com:8445/display/CP/VPC+on+NGDC) are also VPC (not Fabric). Use config context: vpc\_zone\_ctrl\_smart\_nic  and dal4\-qz4\-undercloud for provisioning.  
      \- dal09\-qz4\-sr07\-rk23\-s10  
      \- dal09\-qz4\-sr07\-rk23\-s12  
      \- dal09\-qz4\-sr07\-rk23\-s36  
      \- dal09\-qz4\-sr07\-rk23\-s38

\- mgmt node on rack 48 in Fabric from Rizwan. Use netbox config context: hostos\_uc\_k8s\_mgmt for provisioning.

      \- dal4\-qz4\-sr7\-rk048\-s04 \- 198\.18\.0\.22 \- 198\.19\.0\.22 \- k8s\-worker\-b (uuid: 9a13dff4\-a0da\-4b5f\-a2c0\-67a3de8c5a44\)  
  
**\*PLEASE ANNOUNCE IN \#hostos\-on\-ngdc\-dev IF YOU ARE USING A NODE. Update this table for longer term reservation.**

  


**Getting Access:**

1. [Netbox Onboarding instuctions](https://pages.github.ibm.com/bedrock-squad/ng-fabric-docs/user-onboarding-netbox.html) Get access to Pre\-Prod DCIM. Accept the email access confirmation, then you should be able to login to: [https://us\-east.dcim.test.cloud.ibm.com](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/53/) with you w3 login.
2. [Feddev Jumphost Onboarding](https://pages.github.ibm.com/bedrock-squad/ng-fabric-docs/feddev_jumphost_onboarding.html), get access to 172\.16\.225\.130\. This will allow you to access to the Fabric nodes.   
For nodes that are in VPC you need the following logins:   
labuser1@198\.23\.119\.73   
tmp\_vpc@192\.168\.46\.250 (no\-connection)  
  
tmp\_vpc@172\.16\.225\.134

  
For now, ping Ramkumar Gowrishankar to share the login passwords in Password1 Vault.
3. Use AccessHub to get access to the mascd account. Search for OnePipeline VPC CD Development in AccessHub and select the DevzoneManagement user privilege. Once you have access, login to [IBM Cloud](https://cloud.ibm.com/login). Thanks to Ramkumar for showing us how to do this.
	1. Under User account, select mascd mascd's Account
	2. Click on the resource list icon (three horizontal .\_\_ icon)
	3. Expand Developer tools, select ngdc\-mzone\-management
	4. Under Delivery pipelines select ngdc\-qz4\-fnds
	5. Select Manual\-trigger, this is how to request provisioning. See Provisioning section.
4. Need access to "IBM Cloud \- Nextgen Fabric" to look at provisioning logs (TBD)

**Provisioning:**

Prereq: 

1. New node only: check the switch configuration for [VPC or Undercloud (Fabric)](https://github.ibm.com/neteng/fedramp-cg/tree/28713b0e83d99be8e5f60c97c853920c24172073/input/qz4-dal09) take node of the interface and vlan\_names. The vlan has to be setup correctly in netbox, Organization, Racks, rkxx, your node name, interfaces tab.
2. Each config context in netbox (Other→ Devices) has a section that defines the network configuration. This section must match the interface setting for the node that you are provisioning on. FAST calls this config context as role.
3. Do not edit shared config context, hostos\_uc\_k8s\_mgmt, create a clone of the context that you want to change and make changes to the cloned version.
4. generate an api\-key in pre\-prod netbox
5. Organization, Racks, rkXX, your node name. Find the uuid of your node.

FAST Provisioning with onepipeline 

1. See Getting Access, \#3 mascd account. Follow it to get to manual\-trigger, Run Pipeline. (This run uses FAST DS curl calls.)  

	1. Click Run Pipeline
	2. use {"hostos\_vpc\_zone\_ctrl":\["dal09\-qz4\-sr07\-rk045\-s04"]} for the *role\-hostname,*
	3. Set Failure Threshold to 1,  leave everything else in default.
	4. Watch the run pipeline status for provisioning status

FAST Provisioning from the jumphost, 172\.16\.225\.130:


> ```
> $ curl -k -X 'POST'    '<https://10.203.92.55/api/Fast/provision/nodes>'   -H 'accept: */*'    -H 'Content-Type: application/json'   -H 'Netbox: <your-pre-prod-api-key>'    
> -d '{"qzone": "qz4","nodes": [{"device_uuid": "see Provisioning prereq section #5","role": "hostos_uc_k8s_mgmt"}], "undercloud_config": "dal4-qz4-undercloud"}' | jq .
> ```
> 
> ```
>   
>   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current  
>                                  Dload  Upload   Total   Spent    Left  Speed  
> 100   435    0   279  100   156    158     88  0:00:01  0:00:01 --:--:--   247  
> [  
>   {  
>     "Item1": "31ecf85f-b89b-4ed2-ae5c-d794c2119e3a",  
>     "Item2": "{\"job_id\":\"2024:01:30:20:11:15_9eeaa0a3-1f4b-489c-9893-9cd41f82258d\",\"data\":{\"nodes\":[{\"role\":\"hostos_uc_k8s_mgmt\",\"uuid\":\"9a13dff4-a0da-4b5f-a2c0-67a3de8c5a44\"}],\"context_only\":false},\"error\":null}"  
>   }  
> ]
> ```
>   
> \#\#\#\# using patch\_role to override bundle release version
> 
> $ curl \-k \-X 'POST' '[https://10\.203\.92\.55/api/Fast/provision/nodes](https://10.203.92.55/api/Fast/provision/nodes)' \-H 'accept: \*/\*' \-H 'Content\-Type: application/json' \-H 'Netbox: \<your netbox api key\>' \-d '{"qzone":"qz4","role\_patch": \[{"bundle\_name":"hostos\-config\-release","bundle\_uniqe\_id":"8","bundle\_operation":"","bundle\_version":"6\.1\.31\-kdump\-20240314T213930Z\_dcb7a03"}],"nodes":\[{"device\_uuid":"302fdbae\-2015\-4ec8\-9cba\-c6dcd0eb9b17","role": "nsc\_skip\_post\_config\_hostos"}],"undercloud\_config": "dal4\-qz4\-undercloud\-vmaas"}' \| jq .

  
Take note of Item1 hash. This is your workflow id. To see the status of this provisioning request, run:  
  



> ```
> % wget -q --content-on-error -O- --header="accept: */*"  --header="Content-Type: application/json"  --no-check-certificate     
> --output-document - [https://10.203.92.55/api/Workflow/report/](https://10.203.92.55/api/Workflow/report/31ecf85f-b89b-4ed2-ae5c-d794c2119e3a)<workflow-id> | jq .
> ```

  
If you see ErrorMessage: Devices are part of other active workflow... you can tell it to terminate, run:  
  



> % curl \-k \-X 'POST' "[https://10\.203\.92\.55/api/Workflow/workflow/terminate/](https://10.203.92.55/api/Workflow/workflow/terminate/$1)\<workflow\-id\>" \-H 'accept: \*/\*' \-H 'Content\-Type: application/json'

  


**Inventory:**

* as on *29/02/24* below are nodes available with HOSTOS \-



| **rack** | **hostname(IMS)** | **hostname(netbox)** | **vpcname(netbox)** | **device uuid** | **ip address** | **role/config\_context from****net box** | **undercloud from****net box** | **node personality** | **Notes** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| rack45 VPC | dal09\-qz4\-sr07\-rk045\-s02 | dal2\-qz4\-sr7\-rk045\-s02 | dal09\-qz4\-sr07\-rk45\-s2 | [a32d902e\-9eac\-4805\-9522\-46925b6314ae](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/14435/) | 192\.168\.45\.4 | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/54/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/54/) | [https://us\-east.dcim.test.cloud.ibm.com/login/?next\=/extras/config\-contexts/123/](https://us-east.dcim.test.cloud.ibm.com/login/?next=/extras/config-contexts/123/) | control | hotos dedicated for control |
| rack45 VPC | dal09\-qz4\-sr07\-rk045\-s04 | dal2\-qz4\-sr7\-rk045\-s04 | dal09\-qz4\-sr07\-rk45\-s04 | [bbe42a03\-9d08\-416a\-9ae1\-b819ee0bf255](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/18187/) | 192\.168\.45\.8 | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/93/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/93/) | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/62/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) | compute | hotos dedicated for compute |
| rack48 fabric | dal4\-qz4\-sr7\-rk048\-s04 | k8s\-worker\-01\-b | dal09\-qz4\-sr07\-rk48\-s04 | [9a13dff4\-a0da\-4b5f\-a2c0\-67a3de8c5a44](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/42576/) | 198\.18\.0\.22 | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/38/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/38/) | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/62/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) | mgmt | loaned from Rizwan, used for mgmt |
| rack46 fabric | dal4\-qz4\-sr7\-rk046\-s02 | k8s\-worker\-01\-f | dal09\-qz2\-sr07\-rk046\-sl49 | [78605977\-9ce1\-4399\-af8b\-05bcf897e4b3](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/42575/) | 198\.18\.0\.11 | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/38/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/38/) | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/62/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) | mgmt | loaned from Jose, used for mgmt |
| rack23 VPC | dal2\-qz4\-sr7\-rk023\-s36 | dal2\-qz4\-sr7\-rk023\-s36 | dal09\-qz4\-sr07\-rk23\-s36 | [cd6569eb\-9ccc\-40e7\-96c5\-28fb332d41a2](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/43401/) | 192\.168\.23\.72 | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/41/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/41/) | [https://us\-east.dcim.test.cloud.ibm.com/login/?next\=/extras/config\-contexts/123/](https://us-east.dcim.test.cloud.ibm.com/login/?next=/extras/config-contexts/123/) | control (pensando) | hostos dedicated for Elba |
| rack23 VPC | dal2\-qz4\-sr7\-rk023\-s38 | dal2\-qz4\-sr7\-rk023\-s38 | dal09\-qz4\-sr07\-rk23\-s38 | [2d062065\-8bfd\-48d9\-a7b0\-9fb8c9583a84](https://us-east.dcim.test.cloud.ibm.com/dcim/devices/43402/) | 192\.168\.23\.76 | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/41/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/41/) | [https://us\-east.dcim.test.cloud.ibm.com/extras/config\-contexts/62/](https://us-east.dcim.test.cloud.ibm.com/extras/config-contexts/62/) | control (pensando) | hostos dedicated for Elba |

  


**Helpful Links:**

* `if "WorkflowState":``"`**`NCSFAIL`**`"``,   
"WorkflowErrorDetail": "<workflow id> Failed with Job failed: Error Code: <error_code>.`  
refer for exact error code on this page [https://pages.github.ibm.com/FAST/FAST\-Documentation/support/02\-ncs\-fail/](https://pages.github.ibm.com/FAST/FAST-Documentation/support/02-ncs-fail/)  
if the error code not found on the page contact the NCS team \- [@sprabhu](https://ibmcloudlab.slack.com/team/U01MYFE5P9C) and/or [@Nodas](https://ibmcloudlab.slack.com/team/U039WTYKGDV) \#fabric\-ncs\-network\-public
* [https://pages.github.ibm.com/FAST/FAST\-Documentation/faq/09\-cos\-bundle\-logs/](https://pages.github.ibm.com/FAST/FAST-Documentation/faq/09-cos-bundle-logs/) \- details below
* changes to the jump host access: [https://pages.github.ibm.com/nextgen\-ns3/ns3\_documentation/partners/access\_management/fabric\_jumphost\_pag/](https://pages.github.ibm.com/nextgen-ns3/ns3_documentation/partners/access_management/fabric_jumphost_pag/)

**Accessing deployment Logs from COS bucket:**

Below script can be used to get deployment logs \- 

ensure to update your cos api key, it will prompt for workflow\-id \- 



**get\_deployment\_logs.sh** Expand source



```
date=$(date +"%Y%m%d%H%M%S")
IAM_TOKEN=`curl -s -X POST 'https://iam.cloud.ibm.com/identity/token' -H 'Content-Type: application/x-www-form-urlencoded' -d 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=<your COS api key>' | jq . | grep access_token | awk -F ": " '{print $2}' | tr -d '",'`
read -p "enter your workflowid :" workflowid
key=`curl -s -X "GET" "https://s3.us.cloud-object-storage.appdomain.cloud/vpc-bundle-logs?list-type=2&prefix=${workflowid}" -H "Authorization: Bearer ${IAM_TOKEN}" | sed -n -e 's/.*<Key>\(.*\)<\/Key>.*/\1/p'`
curl -s -X "GET" "https://s3.us.cloud-object-storage.appdomain.cloud/vpc-bundle-logs/${key}" -H "Authorization: Bearer ${IAM_TOKEN}" > ${workflowid}-${date}.log
[[ $? == 0 ]] && echo "logs exported to ${workflowid}-${date}.log"
```


  
1\. **Pre\-requisites**:   
In order to access log files in the COS bucket, one must have an API key. To get an API key created, open a [NextGen Fabric Request Jira](https://jiracloud.swg.usma.ibm.com:8443/secure/CreateIssueDetails!init.jspa?issuetype=14700&pid=18301&components=20203&customfield_10006=FAST-963&summary=FAST-DS%20Support%20Request%20-%20%3CIssue%3E&priority=3) .  


In ticket include the following details.



```
IBM Email Address
Team
Justification for Access
Manager’s approval should be added as a comment to the Jira
```

When processed a FAST team member will reach out to provide your API key. ticket for reference \- [NFR\-2484](https://jiracloud.swg.usma.ibm.com:8443/browse/NFR-2484)

  
2\. **obtain a token** \- 



```
curl -X POST 'https://iam.cloud.ibm.com/identity/token' -H 'Content-Type: application/x-www-form-urlencoded' -d 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=<your COS API key>'

sample output- 
{"access_token":"eyJraWQiOiIyMDI0MDIwNTA4MzgiLCJhbGciOiJSUzI1NiJ9.eyJpYW1faWQiOiJpYW0tU2VydmljZUlkLTE0MzdiNDNkLWM0MzItNDQ1OC04OTllLWMyZmNjZmYwZGQ0MCIsImlkIjoiaWFtLVNlcnZpY2VJZC0xNDM3YjQzZC1jNDMyLTQ0NTgtODk5ZS1jMmZjY2ZmMGRkNDAiLCJyZWFsbWlkIjoiaWFtIiwianRpIjoiYzRkMWM0MWYtNmNiNi00ODQyLTk2YTItYTMzODg2MzEwZTRjIiwiaWRlbnRpZmllciI6IlNlcnZpY2VJZC0xNDM3YjQzZC1jNDMyLTQ0NTgtODk5ZS1jMmZjY2ZmMGRkNDAiLCJuYW1lIjoiaXBvcF9yZWFkZXIiLCJzdWIiOiJTZXJ2aWNlSWQtMTQzN2I0M2QtYzQzMi00NDU4LTg5OWUtYzJmY2NmZjBkZDQwIiwic3ViX3R5cGUiOiJTZXJ2aWNlSWQiLCJhdXRobiI6eyJzdWIiOiJTZXJ2aWNlSWQtMTQzN2I0M2QtYzQzMi00NDU4LTg5OWUtYzJmY2NmZjBkZDQwIiwiaWFtX2lkIjoiaWFtLVNlcnZpY2VJZC0xNDM3YjQzZC1jNDMyLTQ0NTgtODk5ZS1jMmZjY2ZmMGRkNDAiLCJzdWJfdHlwZSI6IlNlcnZpY2VJZCIsIm5hbWUiOiJpcG9wX3JlYWRlciJ9LCJhY2NvdW50Ijp7InZhbGlkIjp0cnVlLCJic3MiOiI1ZWM2MTZhMjMxNTI0OGQxODdhNjJmYTlkZDZmODQxYyIsImZyb3plbiI6dHJ1ZX0sImlhdCI6MTcwOTE5OTY5MSwiZXhwIjoxNzA5MjAzMjkxLCJpc3MiOiJodHRwczovL2lhbS5jbG91ZC5pYm0uY29tL2lkZW50aXR5IiwiZ3JhbnRfdHlwZSI6InVybjppYm06cGFyYW1zOm9hdXRoOmdyYW50LXR5cGU6YXBpa2V5Iiwic2NvcGUiOiJpYm0gb3BlbmlkIiwiY2xpZW50X2lkIjoiZGVmYXVsdCIsImFjciI6MSwiYW1yIjpbInB3ZCJdfQ.HMTYZQKCcd2U45VDJSAt9nbJqeJSCRLAS4TcJPNhbFFV5gnMrF50U7obPoSm5-1yn51OLt8ZYKAVGvXnTSiB4fNxC2gMoQQoWynmVh90XWua7R0uXKnOqqoVSDNvBLaOoU2NnTLKvDvvcY3o-D738q60SHmRkHFlJHgUAPUY0em5ZbzEC7IdLXHvo3anh6GlqoQ71WUupbkpNU3mgwUByHHSI6Xuy5tkz144LRNF6tx5d5lfbFYnlVnk2FoTw0sIRknKmTAIOzMfQjxQV93xZjpuW49YNn29-0b_lkcJHgnBmvyPf3Im31--6C1_Q1YkplgEbMpFwJvsvrJnGQLLDg","refresh_token":"not_supported","token_type":"Bearer","expires_in":3600,"expiration":1709203291,"scope"
docker-na.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local/hostos/hostos-config-release:6.1.31-testngdc18
```

assign the value for access\_token from above command to a variable IAM\_TOKEN for next step  
  
3\. **List objects in bucket** \-



```
curl -X "GET" "https://s3.us.cloud-object-storage.appdomain.cloud/vpc-bundle-logs?list-type=2&prefix=ac65a87c-03de-4a62-8ea7-82252f8354a3" -H "Authorization: Bearer $IAM_TOKEN"

sample output-
<?xml version="1.0" encoding="UTF-8" standalone="yes"?><ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Name>vpc-bundle-logs</Name><Prefix>ac65a87c-03de-4a62-8ea7-82252f8354a3</Prefix><KeyCount>1</KeyCount><MaxKeys>1000</MaxKeys><Delimiter></Delimiter><IsTruncated>false</IsTruncated><Contents><Key>ac65a87c-03de-4a62-8ea7-82252f8354a3-hostos-config-release-ALL</Key><LastModified>2024-02-28T22:24:49.062Z</LastModified><ETag>&quot;2cf13a55d7c2f26b3ab17113d81164d6&quot;</ETag><Size>26654</Size><StorageClass>STANDARD</StorageClass></Contents></ListBucketResult> 
```

capture the key for next step 

  
4\. **Download target log** \-  




```
curl -X "GET" "https://s3.us.cloud-object-storage.appdomain.cloud/vpc-bundle-logs/ac65a87c-03de-4a62-8ea7-82252f8354a3-hostos-config-release-ALL" -H "Authorization: Bearer $IAM_TOKEN"

sample snippet from the output - 
       "perform_config": {
          "task_name": "perform_config",
          "start_time": "2024-02-28 22:21:48.836530",
          "end_time": "2024-02-28 22:23:21.038963",
          "time_delta": "0:01:32.202433",
          "status": "SUCCESS",
          "success": 15,
          "failed": 0,
          "skipped": 10,
          "incompat": 0,
          "tasks": {
            "perform_config_preparechecks": {
              "task_name": "perform_config_preparechecks",
              "start_time": "2024-02-28 22:21:48.836773",
              "end_time": "2024-02-28 22:21:49.513892",
              "time_delta": "0:00:00.677119",
              "status": "SUCCESS"
            },
            "perform_config_coreinfo": {
              "task_name": "perform_config_coreinfo",
              "start_time": "2024-02-28 22:21:49.514291",
              "end_time": "2024-02-28 22:21:53.529373",
              "time_delta": "0:00:04.015082",
              "status": "SUCCESS"
            },
```

  
  
  






#

