---
layout: default
title: Operator API Document 
parent: ops
grand_parent: Developer Information
---


## Operator API actions

**Overview**<br>
This script is designed to interact with a Operator API for managing nodes and virtual machines. The key functionalities include retrieving node information, tainting and untainting nodes, live migrating virtual machines, and checking the status of operator jobs.


**Prerequisites and reference**

https://pages.github.ibm.com/cloudlab/ops-services-docs/docs/getting_started/getting-started.html

**Functionality**

1. Get Information About a Node <br> This option retrieves detailed information about a specified node.

2. Taint a Node <br> This option a taint to a specified node, preventing certain pods from scheduling on it.

3. Live Migrate a Virtual Machine <br> This option initiates the live migration of a specified virtual machine.

4. Untaint a Node <br> This option removes a taint from a specified node, allowing pods to schedule on it again.

5. Check Operator action Status <br> This option retrieves the status of a specified operator job.


---

#### SCRIPT LOCATION
https://github.ibm.com/cloudlab/hostos-misc-tools/blob/master/utils/operator_api/operator_api.sh

