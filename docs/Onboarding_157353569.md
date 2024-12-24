---
layout: default
title: Onboarding  
nav_order: 7
---

## Onboarding

1\. New hire Laptop approval \& W3 credential will be provided by the Manager.(To meet , use Webex).  
  
2\. **Mac management profile reload/reconfigured**

   \- Open terminal on laptop and type \<sudo profiles renew \-type enrollment\>

   \- Mac@IBM can be used to install the required software like slack, yubikey, etc

   \- CISCO AnyConnect VPN can be connected using the IBM W3 credentials.

   **Note:** If you have **Cisco AnyConnect connectivity issue** like **(Certification validation Failure,Authentication Failure,Timeout)** then please go to **Mac@IBM** app 
             store and **Install \& configure** the "**Renew VPN"** app then r**estart the machine** then try to connect 

             <https://mobile.us.ibm.com:15048/tools/vpn/support/support_mac.php>
	     

  
3\. **Email portal \-<https://outlook.office.com/mail/>**

   \-  Log into [Outlook on the web](https://outlook.office.com/) using your w3id and password. If you do not have Outlook installed  [Set Up Outlook](https://w3.ibm.com/#/support/article/outlook_and_apple_mail/setup_outlook?requestedTopicId=setup_outlook),
             [Set Up Apple Mail](https://w3.ibm.com/#/support/article/outlook_and_apple_mail/setup_apple_mail). 
         
   \- If you are already using Outlook [Add your account](https://support.microsoft.com/en-us/office/add-an-email-account-to-outlook-6e27792a-9267-4aa4-8bb6-c84ef146101b#PickTab=Outlook_for_PC).

   \-  We can get support from Apple to setup the required applications and they can be reachable on 080\-49490097 /1\-888\-IBM\-HELP and refer your IBM employee ID.

   \-  For help with Outlook technical issues, the Global Outlook Support Desk is available 24/7 for English\-language calls. Cal] [IT Support (https://w3.ibm.com/#/support)
           and choose option 0 when prompted. 

  
4\. **Slack**

   \- Install slack from Mac@IBM. For Initial access to the app join the **IBM HR** workspace.  
   
   \- Check if you have been added to **IBM cloud platform** workspace, else Dan Wiggins / Manager needs to put forward a request to Kate McCormack for the same. 
            (W3 credential and email access required first)

   \- Any queries or access doubts can be posted to **nextgen\_hostos\_priv**  slack channel in **IBM Cloud Platform workspace** after getting added to it.


5\. **Request for Yubikey**

   step1: [Request a yubikey](https://w3.ibm.com/w3publisher/ibm-cloud-yubikeys)

   step2: Setup

   \- [Register authentication of the yubikey on IBM cloud](https://w3.ibm.com/w3publisher/ibm-cloud-yubikeys/enabling-fido)

   \- Install Yubikey Manager from the Mac@IBM app store

   \- Join the **\#sl\-helpdesk** channel in **IBM Cloud Platform** workspace on slack. (existing member can add the new hire if not able to join directly ).

   \- After joining the channel, send a request on slack to activate yubikey and activate Global Protect VPN.


  
6\. [JIRA Access request process](https://github.ibm.com/genctl/jira-documents-for-cloudlab/blob/main/docs/Permissions_And_AccessHub/Using_AccessHub_To_Request_User_Accounts_For_VPC_NextGen_Applications/README.md)
      

7\. **Github Access** 


  \- Login to GitHub with your IBM intranet ID – you will be logged in automatically→ https://github.ibm.com/cloudlab/

  \- JIRA requests need to raised for orgs:
        1. **[Genctl (GNC)](https://github.ibm.com/orgs/genctl)**
        2. **[Cloud Lab Genesis (CLD)](https://github.ibm.com/orgs/cloudlab/)**


   \- for Genctl org, clone the JIRA : [GNC-8194](https://jiracloud.swg.usma.ibm.com:8443/browse/GNC-8194) 
      follow the summary and description from: [GNC-9903](https://jiracloud.swg.usma.ibm.com:8443/browse/GNC-9903) 
         
   \- for Cloudlab org, clone the JIRA : [CLD-101104](https://jiracloud.swg.usma.ibm.com:8443/browse/CLD-101104) 
      follow the summary and description from: [CLD-164945](https://jiracloud.swg.usma.ibm.com:8443/browse/CLD-164945) 
      
   \- ***Please make sure you keep the description part same except for changing names & assignees. Add a line in the comment section by tagging your manager (@xyz) and 
      request for approval. Assign the "Reporter" to yourself while filling details in JIRA.***



8\. [JfrogArtifactory access](https://github.ibm.com/cloudlab/hostos-docs/blob/docs-r1/docs/developer_information/Repo-Access.md)


9\. **Vault access**
    
   \- Raise JIRA requests for the vault endpoints.
      1. Integration and Development [[DAL QZ2](https://github.ibm.com/gensec/OperatorVault-Public/wiki/Integration-and-Development-Operator-Vault)]
      2. Development [[POK](https://github.ibm.com/gensec/OperatorVault-Public/wiki/POK-Operator-Vault-Endpoint)]

   \- Create a new JIRA under **Security Change Management** project, with subject : Request for Vault access for deployment.

   \- In the description, add the below info:

          - Request for vault access for deployment using DDT tool on development infrastructure.
          - Need access to region <provide #1 or #2 from the above listed endpoints in each ticket>

   \- Sample: [SCM-10904](https://jiracloud.swg.usma.ibm.com:8443/browse/SCM-10904), [SCM-10841](https://jiracloud.swg.usma.ibm.com:8443/browse/SCM-10841)

   \- Add a line in the comment section by tagging your manager (@xyz) and request for approval. 
      Assign the "Reporter" to yourself while filling details in JIRA. After the access is created, respective team will supply the role\_id and secret\_id.
 


10\. **Deployer access and setup**

   \- Go to AccessHub, follow steps in  **[NextGen AccessHub URL](https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest?search=R2VuYmxteEB1cy5pYm0uY29t)** and 
      request access to **IaaS Access Management ,** then request for following groups :

      1. softlayer-shellngqztwo
      2. us-east-shellngqztwo
      3. us-south-shellngqztwo
      4. NG-DEVOPS

   \- After getting the above request approved, install Brew, zsh and teleport on Mac terminal.

   \- For SL Root Cert configuration, go to [DAL QZ2 Deployer Access (Teleport)](https://confluence.swg.usma.ibm.com:8445/pages/viewpage.action?pageId=166502582), follow 
      steps #2 & #3
      
   \- To download PAG follow the doc: [PAG](https://github.ibm.com/genctl/compute-docs/blob/main/docs/Genctl_Network/Genctl_Network_Knowledge_Base/Bye_Bye_Teleport_Welcome_Pag/README.md) 
     

   \- After the "_pag ssh --connect_" step from the above doc, Go to [DAL QZ2 Deployer Access (Teleport)](https://confluence.swg.usma.ibm.com:8445/pages/viewpage.action?pageId=166502582) and follow the step **Add SSH 
      config** for connecting to the deployer.


12\. **Deployer Tool**

   \- [DDT (Discrete Deployer Tool) Setup doc](https://pages.github.ibm.com/genctl/ddtool/)
    
   \- After cloning ddt repo, create a folder "**repos**" in the home directory of deployer and run the below commands inside repos folder.

        1. git clone git@github.ibm.com:genctl/genctl-globals.git
        2. git clone git@github.ibm.com:genctl/telemetry-globals.git
        3. git clone git@github.ibm.com:cloudlab/platform-inventory.git


13\. **onePipeline Access**

   \- Click on [Request New Access](https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest), search for **OnePipeLine**
  
   \- Add it and select **One_Pipeline_Services_Operator** and submit


15\. [Request for a Build Server](https://pages.github.ibm.com/cloudlab/hostos-docs/docs/developer_information/Requesting-a-VM-using-Fyre_201457902.html) (Based on team's requirement)

   \- Mail will be sent with the VM IP and credentials, which can be reset later.


16\. [LogDNA and Sysdig Accesshub](https://github.ibm.com/cloudlab/hostos-docs/blob/docs-r1/docs/developer_information/Cloudlog-access-request.md) access (Based on team's requirement)


17\. [Service Now access request](https://confluence.swg.usma.ibm.com:8445/display/DOC/Getting+access+to+Service+Now), select assignment group as "**is\-ng\-hostos"**


18 . [Getting started on 1password](https://w3.ibm.com/#/support/article/1password_at_ibm/1password_getstarted)


**Product Knowledge Introductory references**

* [NextGen VPC Infrastructure](https://github.ibm.com/cloudlab/srb/blob/master/architecture/hld/README.md)


* [NextGen CI/CD](https://github.ibm.com/cloudlab/srb/tree/master/architecture/cicd)


* [Host OS architecture](https://github.ibm.com/cloudlab/srb/tree/master/architecture/hostos)


* [IBM VPC](https://www.ibm.com/cloud/blog/announcements/next-gen-virtual-servers-vpc)


* [Host os team training](https://ibm.ent.box.com/folder/165986791056?s=ufb04fpbe2vgzfguosesgwpr7ifyumqo)


* [hostos bundle deployment session](https://ibm.ent.box.com/file/763261399075?s=s5w4ywxowa24nuqxc7fype5muitnnfpi)


**Useful Project related Links:**
  

* [Mzone info link](https://github.ibm.com/cloudlab/platform-inventory/tree/master/region)


* [Status of production bundles deployment across all regions](https://9.208.144.219:8060/rbs?mode=last-24)

  







