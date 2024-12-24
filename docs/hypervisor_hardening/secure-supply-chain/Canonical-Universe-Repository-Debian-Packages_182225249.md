---
layout: default
title:  Universe Repository 
parent: Secure Supply chain
grand_parent: Hypervisor Hardening
---

## Universe Repository

Following page is to track Host OS debian packages obtained from canonical universe repos.Namely:

* http://[ports.ubuntu.com/](http://ports.ubuntu.com/) universe
* http://[archive.ubuntu.com/ubuntu/](http://archive.ubuntu.com/ubuntu/) universe

## Canonical Universe Repository Debian Packages - Janmy  

\*Universe packages in use for Ubuntu 22\.04 kernel only

  




| **Package Name** | **Package Version** | **Requirement** | **Notes** | **Notes from Canonical** | **HostOS Updates** |
| --- | --- | --- | --- | --- | --- |
| dropbear | 2020\.81\-5 | Used alongside kdump for retrieving the kernel dump from the crash kernel. | We have a enhacement in progress to check and remove/replace the usage of dropbear | Need to validate that the inclusion of OpenSSH in the initrd with FIPS libraries is possible. |  |
| dropbear\-bin | 2020\.81\-5 | Dependency for dropbear |  |
| busybox | 1:1\.30\.1\-7ubuntu3 | Dependency for kdump tools | kdump\-tools is provided through canonical ppa |
| libtomcrypt1 | 1\.18\.2\-5 | Dependency for dropbear\-bin |  |
| krb5\-user | 1\.19\.2\-2 | Dependency for:  libgssapi\-krb5\-2  libkrb5\-3  libkdb5\-10  libkadm5srv\-mit12  libkadm5clnt\-mit12  libk5crypto3  libgssrpc4 | Kerberos dependencies,suggested dependency | MIR Opened for this at LP\#1986860\. |  |
| libnorm1 | 1\.5\.9\+dfsg\-2 | Dependency for libzmq5 |  | No Alternative. |  |
| libpgm\-5\.3\-0 | 5\.3\.128\~dfsg\-2 | Dependency for libzmq5 |  |
| libzmq5 | 4\.3\.4\-2 | Dependency for python3\-zmq |  |
| python3\-tornado | 6\.1\.0\-3build1 | Dependency for python3\-keylime\-lib | This is part of the dependency for python3\-keylime\-agent which is provided through canonical ppa |
| python3\-zmq | 22\.3\.0\-1build1 | Dependency for:  python3\-tornado  python3\-keylime\-lib |  |
| python3\-gnupg | 0\.4\.8\-1 | Dependency for python3\-keylime\-lib |  |
| python3\-m2crypto | 0\.38\.0\-1ubuntu5 | Dependency for python3\-keylime\-lib |  |
| python3\-py | 1\.10\.0\-1 | Dependency for python3\-zmq |  |
| tpm2\-tools | 5\.2\-1build1 | Dependency for python3\-keylime\-lib |  | Provided through IBM Cloud PPA. |  |
| ntpdate | 1:4\.2\.8p15\+dfsg\-1ubuntu2 | None | Used for setting system time from NTP servers | The ntpdate package has moved to Universe since Bionic.  Use timedatectl or chrony. | Planned to be removed as part of   [CIO\-6755](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-6755?src=confmacro) \- Jira issue doesn't exist or you don't have permission to view it. |
| strongswan\-swanctl | 5\.9\.5\-2ubuntu2 | None | Ipsec utility | This package only implements the swanctl application.  Is it really needed? |  |
| xmlstarlet | 1\.6\.1\-2\.1 | None | Used by zabbix utilities | No Alternative |  |
| libatasmart\-bin | 0\.19\-5build2 | None | Required by Compute | No Alternative |  |
| libnss\-ldapd | 0\.9\.12\-2 | Dependency for nslcd | Used as part of LDAP based system authenticationAdded as part of request   [CIO\-3348](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-3348?src=confmacro) \- Jira issue doesn't exist or you don't have permission to view it.    To support LDAP/AD | The nslcd can certainly be substituted by sssd. |  |
| libpam\-ldapd | 0\.9\.12\-2 |  |
| nslcd | 0\.9\.12\-2 | dependency for libpam\-ldapd |
| traceroute | 1:2\.1\.0\-2 |  | Network debug utility.May be ignored as its not bundled for production | The traceroute utility can be substituted by mtr (on mtr\-tiny package) or tracepath (on iputils\-tracepath package), both in Main. |  |
| ifupdown | 0\.8\.36\+nmu1ubuntu3 | Dependency for:  debianutils  bridge\-utils | Suggested Dependency for bridge\-utils, used as part of network config scripts | Just “Suggests”.  The ifupdown package has been moved to Universe, as we are implementing Netplan as the primary frontend for network configuration. |  |
| python3\-websocket | 1\.2\.3\-1 | Dependency for python3\-docker |  | Packages related to docker are less likely to receive any positive feedback on an MIR. |  |
| python3\-docker | 5\.0\.3\-1 |  | python wrapper used for docker |
| golang\-docker\-credential\-helpers | 0\.6\.4\+ds1\-1 | Dependency for python3\-dockerpycreds |  |
| python3\-dockerpycreds | 0\.3\.0\-1\.1 |  |  |
| libfuse2 | 2\.9\.9\-5ubuntu3 | Dependency for fuse | Storage dependency |  |  |
| fuse | 2\.9\.9\-5ubuntu3 | Dependency for autofs\-config | Storage dependency |  |  |


## Canonical Universe Repository Debian Packages - Bionic 

\*Universe packages in use for Ubuntu 18\.04 kernel only

| Debian Name | Ubuntu Bionic (amd64\) | Ubuntu Bionic (s390x) | Dependencies |
| --- | --- | --- | --- |
| dropbear | 2017\.75\-3build1 | Not Applicable | dropbear\-initramfs dropbear\-run |
| dropbear\-initramfs | 2017\.75\-3build1 | Not Applicable | busyboxdropbear\-bininitramfs\-toolsudev |
| dropbear\-run | 2017\.75\-3build1 | Not Applicable | dropbear\-binlsb\-base |
| dropbear\-bin | 2017\.75\-3build1 | Not Applicable | libc6libtomcrypt1libtommath1zlib1g |
| gnutls\-bin | 3\.5\.18\-1ubuntu1\.5 | 3\.5\.18\-1ubuntu1\.5 | libc6libgnutls\-dane0libgnutls30libopts25libtasn1\-6 |
| krb5\-user | 1\.16\-2ubuntu0\.2 | Not Applicable | libc6libcom\-err2libk5crypto3libkadm5clnt\-mit11libkadm5srv\-mit11libkdb5\-9libkrb5\-3libkrb5support0libss2krb5\-config |
| libintl\-perl | 1\.26\-2build2 | 1\.26\-2build2 | perl |
| libmodule\-find\-perl | 0\.13\-1 | 0\.13\-1 | perl |
| libmodule\-scandeps\-perl | 1\.24\-1 | 1\.24\-1 | perl |
| libnorm1 | 1\.5r6\+dfsg1\-6 | Not Applicable | libc6libgcc1libstdc\+\+6 |
| libopts25 | 1:5\.18\.12\-4 | 1:5\.18\.12\-4 | libc6 |
| libproc\-processtable\-perl | 0\.55\-1 | 0\.55\-1 | perlperl\-baselibc6 |
| libpgm\-5\.2\-0 | 5\.2\.122\~dfsg\-2 | Not Applicable | libc6 |
| libsort\-naturally\-perl | 1\.03\-2 | 1\.03\-2 | perl |
| libterm\-readkey\-perl | 2\.37\-1build1 | 2\.37\-1build1 | perlperl\-baselibc6 |
| libtomcrypt1 | 1\.18\.1\-1ubuntu0\.1 | 1\.18\.1\-1ubuntu0\.1 | libc6libgmp10libtommath1 |
| libzmq5 | 4\.2\.5\-1ubuntu0\.2 | Not Applicable | libc6libgcc1libnorm1libpgm\-5\.2\-0libsodium23libstdc\+\+6 |
| lldpd | 0\.9\.9\-1ubuntu0\.1 | 0\.9\.9\-1ubuntu0\.1 | libbsd0libc6libevent\-2\.1\-6libreadline7libsnmp30libxml2adduserlsb\-base |
| needrestart | 3\.1\-1 | 3\.1\-1 | perldpkggettext\-baselibintl\-perllibproc\-processtable\-perllibsort\-naturally\-perllibmodule\-scandeps\-perllibterm\-readkey\-perllibmodule\-find\-perlbinutilsxz\-utils |
| ntpdate | 1:4\.2\.8p10\+dfsg\-5ubuntu7\.3 | 1:4\.2\.8p10\+dfsg\-5ubuntu7\.3 | netbaselibc6libssl1\.1 |
| python\-websocket | 0\.44\.0\-0ubuntu2 | 0\.44\.0\-0ubuntu2 | python\-sixpython |
| python3\-tornado | 4\.5\.3\-1ubuntu0\.2 | Not Applicable | ca\-certificatespython3libc6 |
| python3\-zmq | 16\.0\.2\-2build2 | Not Applicable | python3libc6libzmq5 |
| strongswan\-swanctl | 5\.6\.2\-1ubuntu2\.7 | 5\.6\.2\-1ubuntu2\.7 | libstrongswanlibc6 |
| tcpd | 7\.6\.q\-27 | 7\.6\.q\-27 | libc6libwrap0 |
| fping | 4\.0\-6 | 4\.0\-6 | libcap2\-binnetbaselibc6 |
| xmlstarlet | 1\.6\.1\-2 | Not Applicable | libc6libxml2libxslt1\.1 |
| libatasmart\-bin | 0\.19\-4 | Not Applicable | libatasmart4libc6 |
| busybox | 1:1\.27\.2\-2ubuntu3\.4 | Not Applicable | libc6 |
| libnftnl7 | 1\.0\.9\-2 | 1\.0\.9\-2 | libc6libjansson4libmnl0 |
| libnss\-ldapd | 0\.9\.9\-1 | Not Applicable | debconflibc6 nslcd |
| liboath0 | 2\.6\.1\-1 | Not Applicable | libc6 |
| libpam\-ldapd | 0\.9\.9\-1 | Not Applicable | libc6 libpam\-runtimelibpam0gnslcd |
| librabbitmq4 | 0\.8\.0\-1ubuntu0\.18\.04\.2 | Not Applicable | libc6 libssl1\.1 |
| nftables | 0\.8\.2\-1 | 0\.8\.2\-1 | dpkglibc6 libgmp10libmnl0 libnftnl7 libreadline7 libxtables12 |
| nslcd | 0\.9\.9\-1 | Not Applicable | debconfadduserlibc6libgssapi\-krb5\-2libldap\-2\.4\-2lsb\-base |
| python3\-bcrypt | 3\.1\.4\-2 | Not Applicable | libc6 python3python3\-cffi\-backend\-api\-maxpython3\-cffi\-backend\-api\-minpython3\-six |
| python3\-cherrypy3 | 8\.9\.1\-2 | Not Applicable | python3python3\-six |
| python3\-gnupg | 0\.4\.1\-1ubuntu1\.18\.04\.1 | Not Applicable | gnupgpython3 |
| python3\-logutils | 0\.3\.3\-5 | Not Applicable | python3 |
| python3\-packaging | 17\.1\-1 | Not Applicable | python3python3\-pyparsingpython3\-six |
| python3\-pecan | 1\.2\.1\-2 | Not Applicable | python3python3\-logutilspython3\-makopython3\-markupsafepython3\-simplegenericpython3\-singledispatchpython3\-sixpython3\-webobpython3\-webtest |
| python3\-simplegeneric | 0\.8\.1\-1 | Not Applicable | python3 |
| python3\-singledispatch | 3\.4\.0\.3\-2 | Not Applicable | python3 |
| python3\-werkzeug | 0\.14\.1\+dfsg1\-1ubuntu0\.1 | Not Applicable | libjs\-jquerypython3 |
| traceroute | 1:2\.1\.0\-2 | Not Applicable | libc6 |
| nvme\-cli | 1\.9\-1 | Not Applicable | libc6 |
| python3\-grpcio | Not Applicable | Not Applicable |  |
| ifupdown | Not Applicable | Not Applicable |  |
| libvirt\-daemon\-driver\-storage\-rbd | Not Applicable | Not Applicable |  |
| python3\-websocket | Not Applicable | Not Applicable |  |
| python3\-docker | Not Applicable | Not Applicable |  |
| golang\-docker\-credential\-helpers | Not Applicable | Not Applicable |  |
| python3\-dockerpycreds | Not Applicable | Not Applicable |  |
| python3\-m2crypto | Not Applicable | Not Applicable |  |
| python3\-py | Not Applicable | Not Applicable |  |
| tpm2\-tools | Not Applicable | Not Applicable |  |





## Attachments:




![](images/icons/bullet_blue.gif)
[image2022\-5\-12\_17\-42\-59\.png](attachments/182225249/308543787.png) (image/png)
   

![](images/icons/bullet_blue.gif)
[image2022\-5\-12\_17\-43\-12\.png](attachments/182225249/308543788.png) (image/png)
   

![](images/icons/bullet_blue.gif)
[image2022\-5\-12\_17\-43\-31\.png](attachments/182225249/308543789.png) (image/png)
   

![](images/icons/bullet_blue.gif)
[image2022\-5\-12\_19\-25\-35\.png](attachments/182225249/308543812.png) (image/png)
   

![](images/icons/bullet_blue.gif)
[image2022\-6\-23\_18\-15\-8\.png](attachments/182225249/326762603.png) (image/png)
   

![](images/icons/bullet_blue.gif)
[image2022\-6\-23\_18\-16\-43\.png](attachments/182225249/326762604.png) (image/png)
   



