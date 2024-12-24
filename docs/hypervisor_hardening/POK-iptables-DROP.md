---
layout: default
title:  POK iptables DROP
parent: Hypervisor Hardening
---


## Temporary/test enforcement of iptables DROP in POK

To this point, HostOS config has NOT been enforcing iptables DROP in POK mzones.     To enforce DROP for your mzone, you must replay HostOS config bundle on your mzone with these changes made to the top of your local undercloud file associated with your mzone.   This is to be used at your discretion for temporary hardening until the HostOS team can create a permanent change to the policy in POK.    Please test in your particular mzone to understand the impact of this hardening to come.  
  
example:

1. find undercloud file

    ```bash
    /bin/grep  "pok1.*undercloud" mzone2411.yml
    undercloud: pok1-qz1-undercloud
      ```

    These changes should go at the top of your local copy of `pok1-qz1-undercloud.yml`

2. Edit undercloud yml

      ```bash
      services:
        iptables:
          forward:
            enforce_node_types:
              - "compute-onepersonality"
          ingress:
            enforce_node_types:
              - "all"
          egress:
            enforce_node_types:
              - "all"
      ```
  
replay HostOS config with DDT.
