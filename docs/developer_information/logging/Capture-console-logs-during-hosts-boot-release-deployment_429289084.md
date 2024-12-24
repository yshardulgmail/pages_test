---
layout: default
title: Capture console logs
parent: Logging
grand_parent: Developer Information
---

# Capture console logs

To enable console logs during the deployment of the hostos\-boot\-release bundle, you can utilize the "\-\-***enableconsolelogs***" feature flag.
By including this flag, the boot logs will be captured.
Without the presence of the flag, the logs will not be recorded. Enabling this feature flag generates a boot log that is stored in the log directory 
as "**ddttool/deploy\_logs/mzoneXXXX/boot\_XXXXX**".

Below is an example usage of the "\-\-***enableconsolelogs***" flag in the hjson file:

```bash
{
  components: {
    hostos: {
      packages: {
        hostos-boot-release: {
          tag: <tag name>
          flags: ["-n <node name> --enableconsolelogs"]
          forcereboot: yes
        }

     }
    }
  }
  mzone: mzoneXXXXX
}

```