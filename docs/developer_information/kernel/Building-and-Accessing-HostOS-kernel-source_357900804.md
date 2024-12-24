---
layout: default
title: Getting the kernel source
parent: Kernel
grand_parent: Developer Information
---
## Getting the kernel source

Canonical does not provide a source debian. Do one of the following 3 ways to get the source code.

**Source from PPA Github:** This is a private PPA, you will likely need to request access. Ask your team lead how.

* IBM Cloud git repositories: [https://code.launchpad.net/\~ibm\-cloud/\+git](https://code.launchpad.net/~ibm-cloud/+git)
* linux\-bionic git repo: [https://code.launchpad.net/\~ibm\-cloud/ibm\-cloud\-private/\+git/linux\-bionic](https://code.launchpad.net/~ibm-cloud/ibm-cloud-private/+git/linux-bionic)
* linux\-jammy git repo: [https://code.launchpad.net/\~ibm\-cloud/ibm\-cloud\-private/\+git/linux\-jammy](https://code.launchpad.net/~ibm-cloud/ibm-cloud-private/+git/linux-jammy)

You have the option to "Browse the code" or clone these repos to your sandbox. Once cloned you can checkout a specific "tag" corresponding to the version you're looking for.

ie: You're looking for linux\-ibm\-gt\-4\.15\.0\-1108\. Run "git tag \| grep 4\.15\.0\-1108". You should get "Ubuntu\-ibm\-gt\-4\.15\.0\-1108\.119".

**Download source from a specific build in the PPA:** login to [Launchpad](https://launchpad.net/). Click on your name on the top right of the browser window. Here you can manage your user profile. Look for "View your private PPA subscriptions".

* Release PPA: as the name suggests, contain kernels we have running in production.
* Build PPA: this has both kernel builds running in production and development kernel builds.

To search for a specific kernel build, click on "View package details" on the right hand side of the browser window. In "Package name contains" field, put in, "linux\-" ***(do not use regular expression.)***  Then select "Any status". Then select either Jammy or Bionic depending on what you're looking for, or "Any Series" if you're not sure. Then click "Filter".

ie: You're looking for linux\-ibm\-gt\-4\.15\.0\-1108\. Scroll down to find and click this version to expand build detail. Under "Package files", download these 3 files:

* linux\-ibm\-gt\_4\.15\.0\-1108\.119\.diff.gz
* linux\-ibm\-gt\_4\.15\.0\-1108\.119\.dsc
* linux\-ibm\-gt\_4\.15\.0\.orig.tar.gz

To prepare the source, run "dpkg\-source \-x linux\-ibm\-gt\_4\.15\.0\-1108\.119\.dsc" This will unpack the tar.gz file and applies diff.gz to create a patched source of linux\-ibm\-gt\_4\.15\.0\-1108\.119\.

*\*\* You may need to refer to Setup Build Environment section below if you don't have pkg\-source command.*

**Download source using pull\-ppa\-source command:**

* ```
export LP_CREDENTIALS_FILE=~/.lpcred
```
* ```
pull-ppa-source -L  --distro=ppa --ppa=ppa:ibm-cloud/release linux-ibm-gt 4.15.0-1108.119 ### on first run, you may get something like this:
```
* ```
root@[2204builder1:/data2#](http://2204builder1/data2) pull-ppa-source -L  --distro=ppa --ppa=ppa:ibm-cloud/release linux-ibm-gt 4.15.0-1108.119  
The authorization page:  
 (<https://launchpad.net/+authorize-token?oauth_token=30lwKFxQBfsGT5V0tghg&allow_permission=DESKTOP_INTEGRATION>)  
should be opening in your browser. Use your browser to authorize  
this program to access Launchpad on your behalf.  
Waiting to hear from Launchpad about your decision...
```
* copy and paste the https:// link above into a web browser, authorize the access. Once completed, come back to your terminal and hit enter to continue.
* You should see the diff, dsc, source tar.gz **AND linux\-ibm\-gt\-4\.15\.0 directory (this is your patched source!)**

\*\* You might need to disable lynx, if you see that text\-based web browser trying to load. Try renaming /usr/bin/lynx to /usr/bin/lynx\-no\-use.

## Setup build Environment

[Request a Fyre VM](https://confluence.swg.usma.ibm.com:8445/display/HCP/Requesting+a+VM+using+Fyre) with the latest Ubuntu, the maximum number of CPUs and storage space allowed. At the time of this writing (Oct 2022\) you can only get up to 8vCPUs and 250GB of diskspace. The more resources you have the faster the build will go. For now, it takes about 90 minutes to build.

* Login and install development packages:
	+ *apt\-get install build\-essential git debhelper dh\-make sbuild\-launchpad\-chroot ubuntu\-dev\-tools devscripts**fakeroot*
	+ *apt\-get build\-dep linux linux\-image\-$(uname \-r)*
* Access to [ibm\-cloud\-private PPA](https://code.launchpad.net/ibm-cloud-private) to be able to clone linux\-jammy repository.

## Prepping kernel source for build

You should pick a name for this build, something short and provides a hint as to what is in the build. We will call it a tag, ie tag: mlxtest4\. Create a directory with this tag and put your kernel source in there.

* *mkdir \~/mlxtest4; cd \~/mlxtest4; git clone git\+[ssh://\<user\-name\>@git.launchpad.net/\~ibm\-cloud/ibm\-cloud\-private/\+git/linux\-jammy](ssh://danymadden@git.launchpad.net/~ibm-cloud/ibm-cloud-private/+git/linux-jammy)*

If you are applying a bug fix patch, then create a clone of the "master" branch. If it is a feature patch, then create your branch off from "master\-next."  


* cd linux\-jammy; git checkout \-b mlxtest4 master

Apply your patch or patches

* git am your.patch1
* git am your.patch4

Check the commit log to make sure that your patches are listed. In this case, I have 4 mlx5 patches that were applied.


```
root@2204builder1:~/jammy/kernel/mlxtest4/linux-jammy# git log --oneline | head  
beaf5794c545 net/mlx5e: multipath, support routes with more than 2 nexthops  
fa810d117cc1 net/mlx5: Lag, add debugfs to query hardware lag state  
5d67c11a7d9e net/mlx5e: multipath, support route update with nexthop id  
3ad6e3c68d36 net/mlx5e: multipath, give priority for routes with smaller network prefix  
b2195463812d UBUNTU: Ubuntu-ibm-gt-5.15.0-1015.17
```
Update the changelog to include your build tag and add any patches that your brought in. There is a way to query the changelog later to see what is in each .deb package. (ASK: Not sure about the command... redhat equivalent: rpm \-qp \-\-changelog \<your.rpm\>)

* vim debian.ibm\-gt/changelog

Your change could look similar to this:

root@2204builder1:\~/jammy/kernel/mlxtest4/linux\-jammy\# git diff  
diff \-\-git a/debian.ibm\-gt/changelog b/debian.ibm\-gt/changelog  
index 65e5ff7a5d3c..1ee2608963d0 100644  
\-\-\- a/debian.ibm\-gt/changelog  
\+\+\+ b/debian.ibm\-gt/changelog  
@@ \-1,4 \+1,8 @@  
\-linux\-ibm\-gt (5\.15\.0\-1015\.17\) jammy; urgency\=medium  
\+linux\-ibm\-gt (5\.15\.0\-1015\.17mlxtest4\) jammy; urgency\=medium  
\+  \* net/mlx5e: multipath, support routes with more than 2 nexthops  
\+  \* net/mlx5: Lag, add debugfs to query hardware lag state  
\+  \* net/mlx5e: multipath, support route update with nexthop id  
\+  \* net/mlx5e: multipath, give priority for routes with smaller network prefix  
  
   \* jammy/linux\-ibm\-gt: 5\.15\.0\-1015\.17 \-proposed tracker (LP: \#1987751\)

  


* The following additional packages may be needed for a successful build  
      apt\-get install debhelper dh\-modaliases   execstack  libpci\-dev  libudev\-dev libssl\-dev libelf\-dev  libiberty\-dev libcap\-dev asciidoc xmlto   
default\-jdk   systemtap\-sdt\-devel libdw\-dev libunwind\-dev libbabeltrace\-dev libzstd\-dev libperl\-dev libslang2\-dev    
  
Since building could take a long time, use "screen" session to ensure that build command will continue if you ssh session dropped. \-S start as session called mlxtest4\-build. To connect to an existing session later "screen \-dr mlxtest4\-build"
* screen \-S mlxtest4\-build

Clean your source tree

* LANG\=C fakeroot debian/rules clean

Build binary and dbgsym packages, log output to build\-mlxtest4\.log. It's a good habit to build dbgsym in case we need it to analyze coredump from an unexpected crash.

* LANG\=C fakeroot debian/rules binary skipdbg\=false 2\>\&1 \| tee build\-mlxtest4\.log

If all goes well, you should have these deb packages in \~/mlxtest4 directory


```
linux-buildinfo-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-headers-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-ibm-gt-cloud-tools-common_5.15.0-1015.17mlxtest4_all.deb  
linux-ibm-gt-headers-5.15.0-1015_5.15.0-1015.17mlxtest4_all.deb  
linux-ibm-gt-tools-5.15.0-1015_5.15.0-1015.17mlxtest4_amd64.deb  
linux-ibm-gt-tools-common_5.15.0-1015.17mlxtest4_all.deb  
linux-ibm-gt-tools-host_5.15.0-1015.17mlxtest4_all.deb  
linux-ibm-gt_5.15.0-1015.17mlxtest4_amd64.tar.gz  
linux-image-unsigned-5.15.0-1015-ibm-gt-dbgsym_5.15.0-1015.17mlxtest4_amd64.ddeb  
linux-image-unsigned-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-modules-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-modules-extra-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-tools-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb
```
## Changing kernel config option

To modify kernel config options, change the following files:


```
   debian.ibm-gt/config/annotations  
   debian.ibm-gt/config/config.common.ubuntu  
   debian.master/config/annotations  
   debian.master/config/config.common.ubuntu
```
Then build as you'd normally build.

## Building hostos\-boot\-payloads

Clone hostos\-boot\-payloads, then create a mlxtest4 branch off of release\-6 branch.

* cd \~/mlxtest4; git clone [git@github.ibm.com](mailto:git@github.ibm.com):cloudlab/hostos\-boot\-payloads.git
* cd hostos\-boot\-payloads; git checkout \-b mlxtest4 remotes/origin/release\-6
* mkdir kernel

Put the following 5 kernel .deb that you just built in into \~/mlxtest4/hostos\-boot\-payloads/kernel


```
linux-modules-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-modules-extra-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-image-unsigned-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-headers-5.15.0-1015-ibm-gt_5.15.0-1015.17mlxtest4_amd64.deb  
linux-ibm-gt-headers-5.15.0-1015_5.15.0-1015.17mlxtest4_all.deb
```
* cd \~/mlxtest4/hostos\-boot\-payloads; make

If build is successful you should see:


```
Generating SHA256 checksum for hostos-boot_jammy-5.15.0.1015-20221005-776cbe1_amd64.yml  
Generating SHA256 checksum for hostos-boot_jammy-5.15.0.1015-20221005-776cbe1_amd64.tgz
```
## Building hostos\-boot\-release

Clone hostos\-boot\-release, then create a mlxtest4 branch off of release\-6 branch

* cd \~/mlxtest4; git clone [git@github.ibm.com](mailto:git@github.ibm.com):cloudlab/hostos\-boot\-release.git
* cd hostos\-boot\-release; git checkout \-b mlxtest4 remotes/origin/release\-6

Download the s390x payloads

* make download;

Put hostos\-boot\* amd64 and s390x payloads into localrepo/

* mkdir localrepo; cd localrepo; cp ../../hostos\-boot\-payloads/output/hostos\* .; cp output/hostos\-boot\_jammy*\*\_s390x.\* .*
* cd ..

Update hostos\-boot\-release.yml to reflect the new jammy version and build harsh. 


```
diff --git a/hostos-boot-release.yml b/hostos-boot-release.yml  
index 08949e7..126f193 100644  
--- a/hostos-boot-release.yml  
+++ b/hostos-boot-release.yml  
@@ -4,8 +4,8 @@ release_manifest:  
   payloads:  
   - payload_type: hostos-boot  
     payload_versions:  
-      amd64: jammy-5.15.0.1013-20220817-17ba7bf  
+      amd64: jammy-5.15.0.1015-20221005-776cbe1  
       s390x: jammy-5.15.0.1015-20220905-776cbe1  
   tools:  
   - tool_name: hostos-boot-tools  
     tool_version: 2.0.0-28ce182
```
  


* make local

You should see something like this at the end of a successful build:


```
Successfully built 76851171c101  
Successfully tagged hostos-boot-release:6.0.0-20221010T212340Z_4b1e1e8
```
## Tagging and pushing release bundle to sandbox artificatory

Now tag and push image ID 76851171c101 to the sandbox artifactory

docker tag 76851171c101 [docker\-na\-public.artifactory.swg\-devops.com/wcp\-genctl\-sandbox\-docker\-local/hostos/](http://docker-na-public.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local/hostos/)[hostos\-boot\-release:6\.0\.0\-20221010T212340Z\_4b1e1e8](http://wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/$2)  
docker push [docker\-na\-public.artifactory.swg\-devops.com/wcp\-genctl\-sandbox\-docker\-local/hostos/](http://docker-na-public.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local/hostos/)[hostos\-boot\-release:6\.0\.0\-20221010T212340Z\_4b1e1e8](http://wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/$2)  
  
That's it. You should test it by deploying this boot\-release bundle, "6\.0\.0\-20221010T212340Z\_4b1e1e8" to a node in an mzone. See complete [hostos release bundles listing for 22\.04](https://confluence.swg.usma.ibm.com:8445/display/HCP/HostOS+on+Ubuntu+22.04).  
  
If deploy is successful, login and check uname \-a output to make sure that you see "mlxtest4" tag that you picked when you built the patched kernel.

## Tips:

* Start with a fresh source from git clone. fakeroot clean doesn't do a thorough job of removing files created from previous build. So your current build may fail. Some of the errors encountered:  

```
mkdir: cannot create directory '/root/jammy/kernel/mlxtest4/linux-jammy/debian/linux-libc-dev/usr/include/x86_64-linux-gnu': File exists  
make: *** [debian/rules.d/[2-binary-arch.mk](http://2-binary-arch.mk):556: install-arch-headers] Error 1  
  
kabi check errors.   
  
mlxtestX tag is not showing up in the .deb name. 
```
* If git am failed, you will need to resolve the conflict manually. I usually do, git am \-\-abort. Then resort to using "patch \-p1 \< path/to.patch" to apply, so that I can see which hunks are failing, then manually merge the failing hunks. double check your merge. git add/commit using the same comment from a given patch, and amend the author.
* [CIO\-8510](https://jiracloud.swg.usma.ibm.com:8443/browse/CIO-8510) Document this page.





## Comments:





| Regarding*apt\-get build\-dep linux linux\-image\-$(uname \-r)*If this fails, using :*apt\-get build\-dep linux linux\-image\-generic*    Posted by Utsav.Shrivastav@ibm.com at Nov 01, 2023 07:44 |
| --- |



 


Document generated by Confluence on Jul 15, 2024 13:04


[Atlassian](https://www.atlassian.com/)


 


