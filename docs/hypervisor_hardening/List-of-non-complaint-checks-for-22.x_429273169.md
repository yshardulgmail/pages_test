---
layout: default
title: CIS non compliant 22.x 
parent: Hypervisor Hardening
---



## HostOS CICD (platform) : List of non\-complaint checks for 22\.x

| **Compliance issue** | **22\.04 status** | **ticket** | **config status** |
| --- | --- | --- | --- |
| 1\.1\.1\.1 Ensure mounting of cramfs filesystems is disabled \- blacklist" | done | [~~CIO\-7752~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-7752) | Done, fix in .112 |
| 1\.1\.10 Disable USB Storage \- blacklist : \[FAILED] | Module is disabled, but there is 'true' in /etc/modprobe.d/usb\-storage.conf | [~~CIO\-7753~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-7753) | Done, fix in .112 |
| 4\.1\.4\.11 Ensure cryptographic mechanisms are used to protect the integrity of audit tools \- auditctl : \[FAILED] | No aide rules | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 4\.1\.4\.11 Ensure cryptographic mechanisms are used to protect the integrity of audit tools \- auditd : \[FAILED] | No aide rules | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 4\.1\.4\.11 Ensure cryptographic mechanisms are used to protect the integrity of audit tools \- ausearch : \[FAILED] | No aide rules | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 4\.1\.4\.11 Ensure cryptographic mechanisms are used to protect the integrity of audit tools \- aureport : \[FAILED] | No aide rules | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 4\.1\.4\.11 Ensure cryptographic mechanisms are used to protect the integrity of audit tools \- autrace : \[FAILED] | No aide rules | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 4\.1\.4\.11 Ensure cryptographic mechanisms are used to protect the integrity of audit tools \- augenrules : \[FAILED] | No aide rules | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 4\.1\.4\.5 Ensure audit configuration files are 640 or more restrictive : \[FAILED] | All files Ok except files in /etc/audit/rules.d/ | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 4\.1\.4\.1 Ensure audit log files are mode 0640 or less permissive : \[FAILED] | All files Ok except files in /etc/audit/rules.d/ | [~~CIO\-8031~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8031) | Done, fix in .112 |
| 2\.2\.16 Ensure rsync service is either not installed or masked : \[FAILED] | Deb installed on all types, but service is set to be stopped. [~~CIO\-1906~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-1906) | [CIO\-8097](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8097) | Done, fix in .113 |
| 4\.2\.1\.5 Ensure journald is not configured to send logs to rsyslog : \[FAILED] | Yes, ForwardToSyslog is set to yes and service is running | [CIO\-8210](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8210) | Done, fix in .115 |
| 4\.2\.2\.5 Ensure logging is configured \- 'mail.\* \-/var/log/mail' | All is set except cron logs | [CIO\-8412](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8412) | Done, fix in .118 |
| 5\.4\.1 Ensure password creation requirements are configured \- 'minlen' |  | [CIO\-8738](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8738) | done, fix in config after .119 |
| 1\.3\.2 Ensure filesystem integrity is regularly checked : \[FAILED] | Yes. We have cronjob that scans at 5am everyday. | [CIO\-8525](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8525) | Done, fix in .121 |
| 4\.2\.2\.5 Ensure logging is configured \- 'auth,authpriv.\* /var/log/auth.log' |  | [CIO\-8412](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8412) | Done, fix in config after .119 |
| 5\.4\.2 Ensure lockout for failed password attempts is configured \- /etc/common\-auth authsucc |  | [CIO\-8766](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8766) | Done, fix in .122 |
| 5\.4\.2 Ensure lockout for failed password attempts is configured \- /etc/common\-auth preauth | No, the file does not have the config recommended. | [CIO\-8766](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8766) | Done, fix in .122 |
| 5\.4\.2 Ensure lockout for failed password attempts is configured \- /etc/security/faillock.conf deny | No, the file does not have the config recommended. | [CIO\-8766](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8766) | Done, fix in .122 |
| 5\.4\.2 Ensure lockout for failed password attempts is configured \- /etc/security/faillock.conf fail\_interval |  | [CIO\-8766](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8766) | Done, fix in .122 |
| 5\.4\.2 Ensure lockout for failed password attempts is configured \- /etc/security/faillock.conf unlock\_time |  | [CIO\-8766](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8766) | Done, fix in .122 |
| 5\.4\.3 Ensure password reuse is limited |  | [CIO\-8834](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8834) | Done, fix in .123 |
| 5\.4\.4 Ensure password hashing algorithm is up to date with the latest standards |  | [CIO\-8993](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8993) | Done, fix in .124 |
| 5\.5\.4 Ensure default user umask is 027 or more restrictive \- Restrictive system umask : \[FAILED] | the config not looks good as per recommendation. | [CIO\-9142](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-9142) | Done. Change is in .125 |
| 6\.2\.9 Ensure root PATH Integrity : \[FAILED] |  | [CIO\-10118](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10118) | Fix is in .133 / .30 |
| 5\.5\.1\.1 Ensure minimum days between password changes is configured \- users : \[FAILED] | No parameter defined | [CIO\-10231](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10231) | Fix is in config .134 / .31 |
| 5\.5\.1\.2 Ensure password expiration is 90 days or less \- users : \[FAILED] | We set to 90 days, which is more secure. | [CIO\-9729](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-9729) | Fix is in .139 |
| 5\.5\.1\.4 Ensure inactive password lock is 30 days or less \- users : \[FAILED] | it is set to \-1 | [CIO\-9729](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-9729) | Fix is in .139 |
| 1\.5\.3 Ensure Automatic Error Reporting is not enabled \- enabled : \[FAILED] | Yes, service is stopped and removed. [~~CIO\-1906~~](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-1906) | [CIO\-8143](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8143) | Done, needs exception |
| 1\.6\.1\.2 Ensure AppArmor is enabled in the bootloader configuration \- apparmor : \[FAILED] | Not applicable as we are using tmpfs fs | [CIO\-8257](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8257) | Done, needs exception |
| 1\.6\.1\.2 Ensure AppArmor is enabled in the bootloader configuration \- security : \[FAILED] | Not applicable as we are using tmpfs fs | [CIO\-8257](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8257) | Done, needs exception |
| 2\.3\.6 Ensure RPC is not installed : \[FAILED] | rpcbind is installed even though not defined in upgrade\-payloads | [CIO\-8132](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8132) | Done, needs exception |
| 1\.4\.1 Ensure bootloader password is set \- 'set superusers' : \[FAILED] | Not applicable as we are using tmpfs fs | [CIO\-8256](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8256) | Done, needs exception |
| 1\.4\.2 Ensure bootloader password is set \- 'passwd\_pbkdf2' : \[FAILED] | Not applicable as we are using tmpfs fs | [CIO\-8256](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8256) | Done, needs exception |
| 1\.1\.9 Disable Automounting : \[FAILED] | rclone actively uses for snapshot uploading and restoring. | [CIO\-8255](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8255) | Done, needs exception |
| 4\.2\.1\.1\.1 Ensure systemd\-journal\-remote is installed : \[FAILED] | deb not installed and we already shipping logs to qradar/logdna | [CIO\-8209](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8209) | Done, needs exception |
| 4\.2\.1\.1\.3 Ensure systemd\-journal\-remote is enabled : \[FAILED] | deb not installed and we already shipping logs to qradar/logdna | [CIO\-8209](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8209) | Done, needs exception |
| 3\.3\.7 Ensure Reverse Path Filtering is enabled \- 'net.ipv4\.conf.all.rp\_filter' (sysctl.conf/sysctl.d) : \[FAILED] | Current values : net.ipv4\.conf.all.rp\_filter \= 0 and net.ipv4\.conf.default.rp\_filter \= 1 | [CIO\-10148](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10148) | needs exception. |
| 3\.3\.7 Ensure Reverse Path Filtering is enabled \- 'sysctl net.ipv4\.conf.all.rp\_filter' : \[FAILED] | Current values : net.ipv4\.conf.all.rp\_filter \= 0 and net.ipv4\.conf.default.rp\_filter \= 1 | [CIO\-10148](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10148) | needs exception. |
| 3\.3\.7 Ensure Reverse Path Filtering is enabled \- 'net.ipv4\.conf.all.rp\_filter' (sysctl.conf/sysctl.d) : \[FAILED] | Current values : net.ipv4\.conf.all.rp\_filter \= 0 and net.ipv4\.conf.default.rp\_filter \= 1 | [CIO\-10148](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10148) | needs exception. |
| 3\.3\.7 Ensure Reverse Path Filtering is enabled \- 'sysctl net.ipv4\.conf.all.rp\_filter' : \[FAILED] | Current values : net.ipv4\.conf.all.rp\_filter \= 0 and net.ipv4\.conf.default.rp\_filter \= 1 | [CIO\-10148](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10148) | needs exception. |
| 3\.2\.2 Ensure IP forwarding is disabled \- ipv4 (sysctl.conf/sysctl.d) |  | [CIO\-10691](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10691) | 1\.)  Widely using ip forwarding in IPtables for Kube networks. ``` -A FORWARD -m comment --comment "kubernetes forwarding rules" -j KUBE-FORWARD  ``` 2\.)  Since we rely on FRR, IP forwarding is required to transist communication between 10\.x and 172\.x(p\* interfaces). ``` root@dal1-qz2-sr2-rk204-s28:~# grep ^'ip forwarding' /etc/frr/frr.conf ip forwarding  ``` 3\.)  For above services to work smoothly, we already have sysctl setting. So, this check needs exception. ``` root@dal1-qz2-sr2-rk204-s28:~# sysctl net.ipv4.ip_forward net.ipv4.ip_forward = 1   root@dal1-qz2-sr2-rk204-s28:~# sysctl net.ipv6.conf.all.forwarding net.ipv6.conf.all.forwarding = 0 ``` |
| 3\.2\.2 Ensure IP forwarding is disabled \- sysctl ipv6 |  | [CIO\-10691](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10691) | Same as above and it needs exception. |
| 3\.2\.2 Ensure IP forwarding is disabled \- sysctl ipv4 : \[FAILED] |  | [CIO\-10691](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-10691) | Same as above and it needs exception. |
| 1\.1\.2\.3 Ensure noexec option set on /tmp partition : \[FAILED] | Can be set noexec |  |  |
| 3\.1\.1 Ensure system is checked to determine if IPv6 is enabled \- sysctl net.ipv6\.conf.all.disable\_ipv6 : \[FAILED] | It is in enabled. Not sure in kube layer and CIS |  |  |
| 3\.1\.1 Ensure system is checked to determine if IPv6 is enabled \- sysctl net.ipv6\.conf.default.disable\_ipv6 : \[FAILED] | It is in enabled. Not sure in kube layer and CIS |  |  |
| 3\.1\.1 Ensure system is checked to determine if IPv6 is enabled \- sysctl.conf net.ipv6\.conf.all.disable\_ipv6 : \[FAILED] | It is in enabled. Not sure in kube layer and CIS |  |  |
| 3\.1\.1 Ensure system is checked to determine if IPv6 is enabled \- sysctl.conf net.ipv6\.conf.default.disable\_ipv6 : \[FAILED] | It is in enabled. Not sure in kube layer and CIS |  |  |
| 3\.3\.8 Ensure TCP SYN Cookies is enabled \- sysctl.conf/sysctl.d : \[FAILED] | Yes, the value set to 1 |  |  |
| 4\.2\.2\.6 Ensure rsyslog is configured to send logs to a remote log host : \[FAILED] | Logs ship to Qradar. But we don't have tuned config with required args |  |  |



 


Document generated by Confluence on Aug 01, 2024 22:21


[Atlassian](https://www.atlassian.com/)


 


