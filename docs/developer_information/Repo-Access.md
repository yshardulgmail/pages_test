---
layout: default
title:  Access HostOS Repos
parent: Developer Information
---

## Process to request Access HostOS Repos

The most common access requests need the following access:

   - wcp-genctl-read - Read-only access to wcp-genctl
   - wcp-genctl-sandbox-read - Read-only access to wcp-genctl-sandbox
   - wcp-genctl-sandbox-write - Write access to wcp-genctl-sandbox
   - wcp-genctl-team-pensando-artifactory-read - Read-only access to wcp-genctl-team-pensando
   - hyc-nextgen-read - Read-only access to hyc-nextgen


1. For **wcp-genctl-platform** repo's  ( used to build hostos bundles )
    - Go to AccessHub (https://ibm-support.saviyntcloud.com/ECMv6/request/requestHome). 
    - Click on "Start an Access Request" -> myself -> Application.
    - Search for "TaaS User Groups" 
    - Click "Request New Account".
    - In the "Groups" card, click "Add".
    - Search for “wcp-genctl-nextgen-team:”
    - Click "Add".
    - Provide a business justification.
    - Click "Done".

2. For **wcp-hostos-production-team** repo's ( ubuntu pro  subscription used for bionic extended support ,  bundles ) 
    - Go to AccessHub (https://ibm-support.saviyntcloud.com/ECMv6/request/requestHome). 
    - Click on "Start an Access Request" -> myself -> Application.
    - Search for "TaaS User Groups".
    - Click "Request New Account".
    - In the "Groups" card, click "Add".
    - Search for “wcp-hostos-production-team:”
    - Click "Add".
    - Provide a business justification.
    - Click "Done".

3. Other commonly requested accounts
    - Go to AccessHub (https://ibm-support.saviyntcloud.com/ECMv6/request/requestHome). 
    - Click on "Start an Access Request" -> myself -> Application.
    - Search for 'VPC Nextgen Artifactory' in the Applications search box
    - Two Resources will show up on the next screen - 'WCP Genctl Artifactory' and 'WCP HYC NextGen Artifactory'.
    - Select the ones you need access to. 'WCP Genctl Artifactory' is the most commonly requested.
    - Click on '**Add**' which will take you to the repositories page.
    - Here '**Select**' the required repositories (wcp-genctl-read/ wcp-genctl-sandbox-read/ wcp-genctl-sandbox-write/ wcp-genctl-team-pensando-artifactory-read )
    - Click on the pencil icon to add the Business Justification. After you're done, hit the same icon again to save your changes.
    - If the access to 'WCP HYC NextGen Artifactory' is also required, follow the same above steps.
    - The request will be routed to your first line manager for review. You can check it in the 'Requests History'.
    - Once the manager reviews the request, the access will be approved or denied.


Write Access Requests

   - The write access to genctl repositories is typically required by the Administrators. 
     Developers should not need to have the write access in normal circumstances.
   - In case you do need to request the write access for the genctl artifactory repositories, or to request 
     the write access for any other repositories not listed under 'Common Access Requests', please follow this process:

     - Create a CIGC JIRA ticket requesting access to the necessary repository. Refer to an example format here. 
     - Explain why you must have the write access.
     - Add label 'CICG_Access_Request' and create the ticket.
     - The ticket will be automatically assigned to the CIGC project owner.
     - Request your manager's approval in the ticket comments.
     - Once approved by your manager, the CIGC project owner will review your request and process or deny it.
     - This write access will automatically get synced with AccessHub.