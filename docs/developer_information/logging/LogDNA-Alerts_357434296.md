---
layout: default
title: LogDNA Alerts 
parent: Logging
grand_parent: Developer Information
---

# LogDNA Alerts

We currently have following filter set for LogDNA which send alerts to slack channel [\#hostosalertnotify](https://ibmcloudlab.slack.com/archives/C041F9D4RFE)

| Filter | Condition | GHE alerts |
| --- | --- | --- |
| app:sys.commands tcpdump (Svc OR Mpls OR eth\+ OR p0\+ OR p1\+ OR mac\+ OR feff\+ OR lo\+ OR ‘\-i’ ) \-('\-r' '.pcap' OR timeout) '\[0]' | Process hung \- Tcpdump. The rule is intended for tcpdump. Tcpdump is supposed to along with timeout command. In the absence of timeout command tcpdump keeps running and hungs when ssh connection times out. The command execution status should be true(0\). | [hostos-tcpdump-without-timeout-cmd](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-tcpdump-without-timeout-cmd.json) |
| hostos\-debug  "boot\-release:1" | Find hostos\-boot\-release 1\.x :The rule is for whenever hosts\-boot\-release with version 1\.x is found in /etc/genesis/release\_bundles. | [hostos-boot-release-1x](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-boot-release-1x.json) |
| app:sys.commands iptables (‘\-A’ OR ‘\-D’ OR ‘\-E’ OR '\-F' OR \-I’ OR ‘\-N’ OR ’\-P’ OR ‘\-R’ OR ‘\-X’ OR ‘append’ OR ‘delete’ OR ‘insert’ OR ‘replace’ OR ‘flush’ OR ‘policy’ OR ‘rename’) ‘\[0]’ | Iptables :The rule is for whenever iptables tables/chains/rules with new/addition/deletion/rename happen, it triggers notification. | [hostos-iptables-changed-cmd](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-iptables-changed-cmd.json) |
| iptables\-monitoring \-transport:audit  \-'KUBE\-' '\-j' (ACCEPT OR DROP) | Iptables :This rule prints the iptables diff change of HostOS chains. | [hostos_debug_iptables_chng](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos_debug_iptables_chng.json) |
| app:sys.commands rmmod \-(‘\-V’ OR version OR ‘\-h’ OR help) ‘\[0]’ | Kernel rmmod:When kernel module is removed by rmmod command, it triggers notification. | [hostos-module-changed-by-rmmod-cmd](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-module-changed-by-rmmod-cmd.json) |
| app:sys.commands insmod \-(‘\-V’ OR version OR ‘\-h’ OR help) ‘\[0]’ | Kernel insmod:When kernel new module is loaded by insmod command, it triggers notification. | [hostos-module-changed-by-insmod-cmd](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-module-changed-by-insmod-cmd.json) |
| app:sys.auth hostos\-monitoring '/usr/sbin/nvme' | NVME hung:When nvme hung process detected by process\-monitoring, it triggers notification. | [hostos-nvme-hung-ps](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-nvme-hung-ps.json) |
| app:sys.commands modprobe \-(‘\-R’ OR resolve OR ‘\-D’ OR show OR ‘\-c’ OR ‘\-n’ OR dry OR ‘\-s’ OR syslog OR ‘\-q’ OR quiet OR ‘\-v’ OR verbose OR ‘\-V’ OR version OR ‘\-h’ OR help) ‘\[0]’ | Kernel modprobe:When kernel modules loaded or unloaded, change in kernel path, and change in module path with modprobe command, it triggers notification. | [hostos-module-changed-by-modprobe-cmd](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-module-changed-by-modprobe-cmd.json) |
| app:sys.journal  syslog_identifier:cron 'syntax error' | Find cron syntax error  :The rule is for whenever cron syntax error is found, it triggers notification. | [cron_syntax_error](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/cron_syntax_error.json) |
| app:sys.journal apparmor DENIED libvirt tap | Find libvirtd apparmor denied error for tap interface :The rule is for whenever libvirtd apparmor denied error for tap interface is found, it triggers notification. | [hostos-libvirt-apparmor-denied](https://github.ibm.com/cloudlab/logging-alerts/blob/main/alerts/HostOS/hostos-libvirt-apparmor-denied.json) |
