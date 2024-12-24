---
layout: default
title: Update Cpio
parent: Kernel
grand_parent: Developer Information
---
# Update Cpio 
 
To provide a mechanism for taking an existing  canonical image file (.cpio file) and injecting it with a new linux kernel and creating an update .cpio file.  This new file can then be incorporated using NextGen tools, into a hostos\-boot\-release bundle and tested on an Mzone without having to round trip the kernel files through Canonical.  The idea is this will allow IBM Linux teams to test and verify new kernels quickly and with much less overhead than having to pass the files back to Canonical and await an cpio image with those new kernel packages installed.

  


## **Inputs:**The following  components are required to complete this process.

1. Canonical scripts for mounting and updating a cpio image   (image\_tools.tgz)
2. Linux Debian packages for the new kernel.
3. An existing cpio.gz compressed file that contains an older kernel.
4. A variation of the hostos\-boot\-payloads tooling that with a single manifest containing the list of Debian kernel packages.

  


## **Process:**The steps below detail what needs to be done to create the new cpio.gz file with the update kernel.

1. Utilize the hostos\-upgrade\-payloads tooling to create a tar ball (tgz) that has a local apt repository containing the linux kernel packages and all dependencies.
2. On a bare metal machine ( suggest using an isolated node in an Mzone), copy the following components:
	1. The cpio.gz file with the old kernel. \- the current inventory of such files can be found here:
	2. The canonical tools (image\_tools.tgz)
	3. The tarball created in step 1\.
3. Unroll the image\_tools.tgz file  \- this contains the following: **mount\-image.sh**

**umount\-image.sh**

[README.md](http://README.md)
4. Unroll the tar ball created in step 1\.  This will set up a local apt repository containing the linux kernel debian packages.
5. The README.md file in the image\_tools file contains instructions on how to use the scripts to mount the cpio image and update it.  We will illustrate with an example:
6. Create a directory for the mount point (e.g. $ mkdir /tmp/my\-image)
7. Unzip the cpi.gz with the old kernel : (e.g. $ gunzip bionic\-image.cpio.gz)
8. Use the canonical mount\-image $ sudo ./mount\-image bionic\-image.cpio /tmp/my\-image
9. Copy the directory for the local apt repo containing the linux kernel debian packages to the mount point (e.g. cp /tmp/kernel\-apt /tmp/my\-image/tmp)
10. chroot into the mounted image and edit the /etc/apt/source.list or add an entry to the /etc/apt/sources.list.d directory to point to the new apt repo.
11. update the mounted image (e.g. sudo chroot apt\-update \-y)
12. follow the instructions here to fix the grub stuff:   
[http://edoceo.com/notabene/grub\-probe\-error\-cannot\-find\-device\-for\-root](http://edoceo.com/notabene/grub-probe-error-cannot-find-device-for-root)
13. install the new linux\-image for with the new kernel (e.g. sudo chroot apt install linux\-image )
14. use the canonical tools to unmount and save the cpio file (e.g.  sudo ./umount\-image /tmp/my\-image new\-image.cpio )
15. gzip up the cpio file and copy to the artifactory repo where these are stored (currently:[https://na.artifactory.swg\-devops.com:443/artifactory/](https://na.artifactory.swg-devops.com:443/artifactory/)wcp\-genctl\-platform\-canonical\-images\-generic\-local/)
16. Create a new hostos\-boot\-payload from the update cpio file.
	1. ```
	git clone git@github.ibm.com:cloudlab/hostos-boot-payloads.git  
	git checkout wolf  
	cd ./hostos-boot-payloads  
	make build
	```
17. Make a new hostos\-boot\-release bundle from the new xcatfs file build in step 15\.
18. Test the new hostos\-boot\-release bundle.  (i.e. copy it to a deployer server, load it in to the docker registry and use the xcat docker command to boot  the nodes and verify they come up with the updated kernel!

