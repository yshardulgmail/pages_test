---
layout: default
title: Break Glass root access
parent: Debug
grand_parent: Developer Information
---
## "Break Glass" procedure for gaining root access

## Audience

Support engineers who need to log into the console of an mzone node.

## Purpose

The purpose of this document is to provide the information needed to enable users to obtain the root password for an mzone node, and rotate a new password into vault
This includes:

* Running a utility to retrieve the current root password from vault
* Creating an emergency CR to have a new password "rotated" into vault.

## Scope

The scope is limited to instruction on how to retrieve the password, and how to create a CR to change the password to an unknown value.

**This applies only to NG.** As of this writing, root password automation is not implemented in NGDC. Management of the root password is left to the system owner(s) in NGDC.

## Prerequisites

To complete this, you will need:

* "deploy" access to vault`
* Access to ServiceNow to create a CR
* Access to the 1Password vault **Hostos CORE IPOPS Data**
* Knowledge and/or documentation on how to use the password to log into the console of an mzone node

## Procedure

1. Download the script
1. Generate a vault token
2. Run the script
3. Create the CR
4. Remove the script
## Steps
1. **Download the script**
   1. Log into 1Password,
   2. Find the vault "**Hostos CORE IPOPS Data**"
   3. Download the script  **rpw.sh**
   4. `chmod +x rpw.sh`
1. **Generate a vault token**
   1.  Use your role\_id and secret\_id to generate a token in vault.
1. **Run the script**
   1. `./rpw.sh` <br>
   The script will prompt you for your vault token, then retrieve the password and display it for you. 
1. **Create the CR**
   1. When you run the above script, it will instruct you to create a CR, and it will provide the `vaultctl` command for the vault team to run.
   2. Create a CR following the process [here](https://github.ibm.com/gensec/OperatorVault-Wiki/wiki/How-To:-Submit-a-Production-or-Staging-Vault-Secret-Change-Request)
   3. Follow: "When opening a CR which needs to be executed in less than 4 days"
 1. **Remove the script**  
     `rm rpw.sh`
 <hr>

**NOTE: The incident that related to this activity must not be closed until the CR is approved.**
