---
layout: default
title:  Reboot mcdvm check
parent: NGDC
grand_parent: Developer Information
---
# Reboot mcdvm check

These need to happen in a vm reboot.

1. Validate /boot/grub/grub.cfg has proper menuentry and initrd before reboot
2. Reboot
3. Remove /etc/genesis/*md5sum,b64
4. Provision post-config

## Investigation

- What are the services running pre-reboot
- What are the services running post-reboot (before installing post-config again.) Can we skip running post-config after reboot?

Current check (pre-reboot and post-reboot - comparison results?)

```bash
mkdir /tmp/pre
cat /etc/passwd > /tmp/pre/etc_passwd
cat /etc/shadow > /tmp/pre/etc_shadow
cat /etc/group > /tmp/pre/etc_group
cat /etc/gshadow > /tmp/pre/etc_gshadow
cat /boot/grub/grub.cfg > /tmp/pre/boot_grub_grub.cfg
cat /etc/resolv.conf > /tmp/pre/etc_resolv.conf
cat /etc/hosts > /tmp/pre/etc_hosts
ip addr sh > /tmp/pre/ip_addr_sh
route -nv > /tmp/pre/route_nv
sysctl -a > /tmp/pre/sysctl_a
cat /etc/frr/frr.conf > /tmp/pre/etc_frr_frr.conf
cat /etc/frr/vtysh.conf > /tmp/pre/etc_frr_vtysh.conf
timedatectl > /tmp/pre/timedatectl
ipset save > /tmp/pre/ipset_save
iptables-save > /tmp/pre/iptables-save
dpkg -l > /tmp/pre/dpkg_l
systemctl list-units --type=service --no-pager > /tmp/pre/systemctl_services
lsmod > /tmp/pre/lsmod
apparmor_status > /tmp/pre/apparmor_status
cat /etc/genesis/network.conf > /tmp/pre/genesis_network.conf
cat /etc/genesis/release_bundles > /tmp/pre/genesis_release_bundles
auditctl -s > /tmp/pre/auditctl_s
df -hT > /tmp/pre/df_hT
cat /etc/fstab > /tmp/pre/etc_fstab
cat /etc/mtab > /tmp/pre/etc_mtab
/opt/nessus_agent/sbin/nessuscli agent status > /tmp/pre/nessus_agent_status
kdump-config show > /tmp/pre/kdump-config
uname -a > /tmp/pre/uname_a
cat /proc/cmdline > /tmp/pre/proc_cmdline
lscpu > /tmp/pre/lscpu
free -g > /tmp/pre/free_g
chage -l root > /tmp/pre/chage_root
chage -l sysop > /tmp/pre/chage_root
grep -v ^# /etc/sudoers > /tmp/pre/etc_sudoers
visudo -c > /tmp/pre/visudo_c
cat /etc/securetty > /tmp/pre/etc_securetty
cat /etc/security/access.conf > /tmp/pre/etc_security_access.conf
grep -v ^# /etc/security/limits.conf > /tmp/pre/etc_security_limits.conf
grep -v ^# /etc/security/pwquality.conf > /tmp/pre/etc_security_pwquality.conf
all cron related files
cat /etc/tenable_tag > /tmp/pre/etc_tenable_tag
cat /etc/timezone > /tmp/pre/etc_timezone
cat /etc/nsswitch.conf > /tmp/pre/etc_nsswitch.conf
numactl -H > /tmp/pre/numactl_H
grep -v ^# /etc/ssh/sshd_config > /tmp/pre/etc_ssh_sshd_config
cat /etc/pam.d/common-account > /tmp/pre/etc_pam.d_common-account
cat /etc/pam.d/common-auth > /tmp/pre/etc_pam.d_common-auth
cat /etc/pam.d/common-session > /tmp/pre/etc_pam.d_common-session
grep -v ^# /etc/pam.d/sshd > /tmp/pre/etc_pam.d_sshd
ls /etc/modprobe.d/ > /tmp/pre/ls_etc_modprobe.d
cat ~sysop/.ssh/authorized_keys > /tmp/pre/home_sysop_ssh_authorized_keys
hostname > /tmp/pre/hostname
netstat -lntp > /tmp/pre/netstat_lntp
```
