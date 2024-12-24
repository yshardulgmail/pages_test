---
layout: default
title: Log Permission 
parent: Logs 
grand_parent: Hypervisor Hardening
---

## Upstream wrong permissions for /var/log/apt/history.log

investigating `/var/log/apt/history.log` in related VULN ticket particularly, I learned that the permissions on */var/log/apt/history.log* are set to 644 which is world\-readable. If I manually change permissions on this log file to 640 apt changes it back to 644 the next time it adds to the file.

It looks to me like this happens in the source code here:  
[https://git.launchpad.net/ubuntu/\+source/apt/tree/apt\-pkg/deb/dpkgpm.cc\#n1042](https://git.launchpad.net/ubuntu/+source/apt/tree/apt-pkg/deb/dpkgpm.cc#n1042) 

with this line in [dpkgm.cc](http://dpkgm.cc):

```bash
chmod(history_name.c_str(), 0644);
```

It is important to ensure that log files have the correct permissions to ensure that sensitive data is archived and protected.  
The info in this file shows which packages/versions have been installed/uninstalled, and could be useful to a hacker to determine vulnerabilities.

The permissions on this file being set to 644 causes systems to fail industry standard CIS Security Benchmarks \[4\.2\.3 Ensure permissions on all logfiles are configured]

case was created with canonical to have this fixed upstream. 

Case [00344508](https://portal.support.canonical.com/ua/s/case/5004K00000O3sTJQAZ) to track this with Canonical.

## FileCreateMode in syslog.conf or 'Create' in logrotate.conf

After rotation (before the postrotate script is run) the log file is created (with the same name as the log file just rotated). mode specifies the mode for the log file in octal, owner specifies the user name who will own the log file, and group specifies the group the log file will belong to. Any of the log file attributes may be omitted, in which case those attributes for the new file will use the same values as the original log file for the omitted attributes.

Use it like so:

```bash
*/var/log/maillog {*  
*....*  
*create 640 user group*  
*....*  
*}*  
```   
so we do this either in /etc/logrotate.conf or a separate file in /etc/logrotate.d ensuring no other file already overrides this.  
this can be handled in the rsyslog configuration /etc/rsyslog.conf using FileCreateMode parameter. 

FileCreateMode 0640

## Overrride initial permissions set by Debian


To ensure CIS benchmark 4\.2\.3 standards as met all logs under /var/log should be 640\. 

However, the permissions on few files are being set to 644 which causes systems to fail industry standard 

CIS Security Benchmarks \[4\.2\.3 Ensure permissions on all logfiles are configured]

Example \- 

```bash
The command 'OUTPUT=$(ls -l /var/log); /usr/bin/find /var/log -type f -perm /g+wx,o+rwx -ls | /usr/bin/awk -v awkvar="${OUTPUT}" '{print} END {if (NR == 0) print awkvar "\npass" ; else print "fail"}'' returned : 

30782      0 -rw-rw----   1 root     utmp            0 Nov  1 00:00 /var/log/btmp
   103273      0 -rw-rw-r--   1 root     utmp            0 Nov  1 00:00 /var/log/wtmp
    43734     56 -rw-rw-r--   1 root     utmp     18704352 Nov  2 23:46 /var/log/lastlog
 fail
```

the default permissions for wtmp, btmp and lastlog are defined in /usr/lib/tmpfiles.d/var.conf

```bash
root@dal2-qz2-sr2-rk118-s12:/usr/lib/systemd# cat /usr/lib/tmpfiles.d/var.conf | grep tmp
# See tmpfiles.d(5) for details
f /var/log/wtmp 0664 root utmp -
f /var/log/btmp 0660 root utmp -
f /var/log/lastlog 0664 root utmp -
```

since this is a systemd component, we can copy /usr/lib/tmpfiles.d/var.conf to /etc/tmpfiles.d/ and modify the copy to override the default permissions.

```bash
root@dal2-qz2-sr2-rk118-s12:/etc/tmpfiles.d# dpkg -S /usr/lib/tmpfiles.d/var.conf 
systemd: /usr/lib/tmpfiles.d/var.conf
```

any file in /etc/tmpfiles.d whose name will exactly match a file in /usr/lib/tmpfiles.d/ will override /usr/lib/tmpfiles.d/ version of the file. 

This is the standard way to customize systemd services in a way that is guaranteed not to be overwritten with debian updates.

Test results \- 

```bash
root@dal2-qz2-sr2-rk118-s12:~# ls -lrt /etc/tmpfiles.d/var.conf 
-rw-r--r-- 1 root root 569 Nov  4 15:36 /etc/tmpfiles.d/var.conf
 root@dal2-qz2-sr2-rk118-s12:~# cat /etc/tmpfiles.d/var.conf | grep tmp
# See tmpfiles.d(5) for details
f /var/log/wtmp 0640 root utmp -
f /var/log/btmp 0640 root utmp -
f /var/log/lastlog 0640 root utmp -

root@dal2-qz2-sr2-rk118-s12:~# ls -lrt /var/log/wtmp
-rw-r----- 1 root utmp 2688 Nov  4 16:33 /var/log/wtmp
root@dal2-qz2-sr2-rk118-s12:~# ls -lrt /var/log/btmp
-rw-r----- 1 root utmp 0 Dec 15  2021 /var/log/btmp
root@dal2-qz2-sr2-rk118-s12:~# ls -lrt /var/log/lastlog
-rw-r----- 1 root utmp 17520292 Nov  4 16:33 /var/log/lastlog
```

above suggests that the default permission was overridden and files were created with permission 640 which ensures CIS benchmark 4\.2\.3 standards. 

  
