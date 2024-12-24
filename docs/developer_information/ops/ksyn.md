---
layout: default
title: Ksync 
parent: ops
grand_parent: Developer Information
---


## Overview


Everything you ever wanted to know about`ksync`, but didn't know who to
ask.
The ksync script is a wrapper for the kuserath tool that was initially
supplied by Alex Iannicelli years ago. To understand ksync thoroughly, you
need to know a little about kuserath. (The original script has evolved
considerably.)

It is covered in part here [How to get around in the Traceability
world](https://confluence.swg.usma.ibm.com:8445/display/OP/How+to+get+around+in+the+Traceability+world)

## Audience

Support personal with an in depth knowledge about the workings of bash, ssh
and Kubernetes.


#### kuserath:

This script is designed to create a "clusterrolebinding" in Kubernetes, for a
specific user, in a specific mzone. To the end user, they would say it gives
them a "Kubernetes _context_ ".

In order to do this, admin access is required. This is accomplished by
utilizing the functional id `vpcoppr`.

#### How it works: 

- Create an openssl key,
- Create a CSR using that key
- Use the CSR to generate a cert in Kubernetes
- Use the cert to create a clusterrolebinding in Kubernetes

The above process creates configuration and cache information in
**`~/.kube`**. It also stores the cert info in **`~/.certs`**

#### kync:

This is a bash script that is maintained in /usr/bin/ksync.

It accepts a list of one or more mzones on which to create clusterrolebindings
(AKA "contexts".)

```
$ ksync -h

Script Usage: ksync -m mzone[,mzone2,mzone3] [-y] [-i] [-s] [-h]

Where:
 -m One or more mzones on which to set up a clusterrolebinding (AKA "context") for yourself`  
 -y Assume "yes" to all user prompts
 -i Ignore all errors. (Useful when processing multiple mzones)
 -s Set your kube context to point to the last mzone in the list provided via
 -m 
 -h Show this help
```
  
For each mzone specified, it:

- Ensures a valid "mzone.yml" exists in `/var/opt/platform-inventory/region/`.
- Retrieves the Kubernetes master IP address from that mzone.yml.
- Ensures vpcoppr has a clusterrolebinding in the specified mzone. If a new one is required, vpcoppr needs to log into the master node and create itself a clusterrolebinding as the kube admin. This requires sudo access on the mzone node, and therefore must be done as user `sysop`. ksync accomplishes this "remote execution" by calling kuserath with the**`-r`** flag
- Call kuserath to create the user's clusterrolebinding.

## What can go wrong

If you didn't read the above explanation, this may not make sense to you.

1. No valid mzone.yml

    ```
    $ ksync mzone1234  
    Processing mzone1234 . . .  
    Could not find readable mzone.yml file "/var/opt/platform-
    inventory/region/mzone1234.yml" for mzone "mzone1234" (Possible permission
    problem?)
    ```

    Cause: Either:

    - The mzone.yml does not exist and _needs to be created_ ,  
    - Permission got corrupted and an _SRE Jira ticket needs to be created to have permission addressed  

2. Bad cert

    ```
    $ ksync mzone729 Processing mzone7295 . . .  
    Unable to connect to the server: x509: certificate signed by unknown authority
    (possibly because of "crypto/rsa: verification error" while trying to verify
    candidate authority certificate "kubernetes")  
    "kubectl get nodes" is not working for vpcoppr. Attempting to fix that. . .  
    Just a moment please.
    ```

    Cause: A cert has gone stale - typically because the mzone or Kubernetes was
    redeployed.  
    At this point, ksync is attempting to correct the issue, which is often
    successful.  
    If it hangs at that point, the cert may look valid but still be stale. Press
    CTRL-C and rerun ksync with the -x flag to see where it fails. e.g. `bash -x
    ksync mzone1234`  
    Doing this will allow you to determine if the problem is with vpcoppr's cert
    or your cert.  

    - Go to the` $HOME/.certs `directory of the appropriate user (you or vpcoppr) and remove all artifacts associated with the mzone that is failing, e.g. rm *mzone1234*  
    - rerun ksync  

    In rare occasions, that is not successful. In these cases, it is necessary to
    "wipe the slate clean" by removing all Kubernetes artifacts. See step 3 below

3. Weird behavior not documented above:  

    - Remove all Kubernetes artifacts This is a drastic measure. It will remove all a user's contexts, and should only be done as a last resort.   
    cd to the HOME of the user in question (you or vpcoppr)  
    ```
    rm -r .certs/* .kube/*  
    ```
  
4. sudo issues for `vpcoppr`: 
    ```
    ksync mzone729
    Processing mzone729 . . .
    Error in configuration: context was not found for specified context: vpcoppr-dal4-rk101-s06-mzone729
    "kubectl get nodes" is not working for vpcoppr. Attempting to fix that. . .
    Just a moment please.
    [sudo] password for vpcoppr:
    [sudo] password for vpcoppr: 
    ```
    This occurs when the automation user `vpcoppr` does not have sudo permission.
    Three things to check:
    1. Is `vpcoppr` in the *deploy* Linux group?
    2. Does the *deploy* group exist in `/etc/group` ?
    3. Does the file `/etc/sudoers.d/10-deploy` exist ? 
   
    ```
    $ groups vpcoppr
    vpcoppr : vpcoppr adm docker deploy
    
    $ grep deploy /etc/group
    deploy:x:515:zabbixagent,vpcoppr

    $ sudo cat /etc/sudoers.d/10-deploy
    %deploy ALL=(ALL) NOPASSWD:ALL

    ```
5. If that doesn't work:

    - If none of the above seems to work, you may need to become vpcoppr ( `sudo su
    vpcoppr` ) and interactively run the commands you observed when you ran
    ksync with the `-x` flag. This may include running the embedded `kuserath`
    commands with the `-x` flag.
