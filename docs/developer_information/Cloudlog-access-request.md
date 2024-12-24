---
layout: default
title:  Request yubikey
parent: Developer Information
---

# Cloud Log access and help with Yubikey

**Summary**
  - This is a collection of steps and information (subject to change without notice) on the current process for someone to request Cloud Log access (previously LogDNA) and help with their YubiKey.

**YubiKey Info**
  - In order to have a valid/working YubiKey you also need a valid/working SoftLayer (SL) account.   If you are unsure if you have one, or it's not working, post a request for help in the Slack channel listed below.

  - Slack help channel #sl-helpdesk 

**BNPP Cloud Log Access**

  - This is a 2 part process.
     1) request access to the BNPP IAM account
     2) request access to the BNPP Cloud Log instances:
  STEP 1:
  - Go to https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest, In the search box under ALL APPLICATIONS search for BNPP
  - Select "BNPP Cloud Account" -> "ADD TO CART" -> "NEXT" -> "I AGREE"
  - Under "Business Justification", add Require Cloud Log instance access in order to debug automated test failures" -> SUBMIT
  STEP 2:
  - After getting the above request approved, go to https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest
  - In the search box under ALL APPLICATIONS search for BNPP -> "BNPP Cloud Account" -> "MODIFY ACCOUNT" -> "ADD TO CART" -> "CHECKOUT"
  - Under "Available Group Name", for "Access BNPP resources for ACS", click the +ADD button
  - Click "NEXT" -> check box "I AGREE", provide the above justification and SUBMIT.

**Genesis_Prod Cloud Log Access**

  - Go to https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest, search NextGen(Genesis@us.ibm.com).
  - click on Request New account -> add -> select the required logDNA group name and provide justification.
  - Submit the request and once its approved after 24 - 48 hours , check your email for an invitation from no-reply@ibm.cloud.com.  
  - The Email subject will be titled with something similar to: **Account: Action required: You are invited to join an account in IBM Cloud.**
  - Click the 'Join Now' link in the email to connect your ibm cloud account to the nextgen account for prod logdna access

**New "NextGen" Staging Request Process**

  - Go to https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest, Search for "VPC NextGen Staging Account"
  - please select the one with the description "Staging account for VPC NexGen that will contain cluster access and LogDNA and Sysdig instances"; click the "Request New Account"
  - From the "Group_Name" panel, click "Add" and select the group "developer_staging_read_access", once added click "Review" to proceed (if you cannot find this group, please select the logdna_staging_all_reader group)
  - Enter business justification and click "Submit"

**NOTES**
  - This procedure is not instant; you need to wait for all the approvals to be processed first, and then allow AccessHub to sync with the cloud account. 
  - To check this, go into AccessHub and review your request; expand all the access groups and ensure that the approvals are processed and the task is GRANTED.
  - If the task is PENDING, you either need to wait or remove your access and re-try.
  - If you have checked your email and have not received any invitations, reach out to Allan Lew and see if the invitation can be re-sent. 
  - There may be delays in this step, as there are always other priorities.

**FRA-PROD (eu-fr) Cloud Log Access**

  - Go to https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest , click on "ViewExistingAccess" on the left panel.
  - Select APPLICATION , in the drop-down Select NextGen(Genesis@us.ibm.com).
  - Check the account details for Entitlements listed in the Entitlement Value column if they have below:
           - Activity Tracker with LogDNA (prod and staging) 
           - IBM Sysdig access to staging and production 
           - IBM Sysdig for development team 
           - LogDNA for developers
  - If you do have any of these entitlements, and need access to Cloud Logs,
    activity tracker or sysdig in Frankfurt (EU regions) please perform the following to request access: 
  - Select NextGen(Genesis@us.ibm.com) in the https://ibm-support.saviyntcloud.com/ECMv6/request/applicationRequest -> MODIFY EXISTING ACCOUNT.
  - Select the checkout button and add the  one of the following roles:  
           - For Advanced Customer Support team --> Access EU resources for ACS 
           - For Developers (most people) --> Access EU resources for development 
           - For Site Reliability Engineer/Operations teams (Roy/Majad's org) --> Access EU resources for SRE/Operations 
  - In the Request Details click on the I agree checkbox -> next -> provide business justification -> submit
  - **Note** : For access to Madrid (MAD),make sure to add "VPC development team access to logDNA and Sysdig instances in EU" role while modifying account.
