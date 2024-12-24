---
layout: default
title:  Local deb Payload 
parent: Build
grand_parent: Developer Information
---


# Add Local Deb to Upgrade Payload and Release Bundle 

* This page provides an example of building an upgrade payload with a local debian package
* This page will use local builds of payloads and release bundles.

## Overview

* Generally, most of the hostos repos support building their artifact with local contents for development and test purposes.
* Examples would be upgrade payloads with one or more local debian packages, or release bundle builds using local payload versions.
* The common pattern for this across most hostos repos is to put the local content into a directory called 'localrepo'.
* Either a specific build target such as 'make local' or the normal make target will prefer content from the localrepo directory over what is available in artifactory.

## Example: Build local base\-net\-sw\-release bundle containing local deb and payload

### Build new base\-net\-sw\-payload

* here we obtain the local deb, update the manifest, and build the payload



**Build Payload** 



```bash
#copy the deb to localrepo
> scp linuxvm:cloudnet/mlx-firmware_2.245.0_all.deb /Users/aross/hostos/upgrade-payloads/localrepo/
mlx-firmware_2.245.0_all.deb

#update manifest for the version of this local deb
> git diff HEAD~1
--- /var/folders/3r/b9s5n1wn3y92ynzzzy9p9sq40000gn/T//BJPs49_base-net-sw.yml    2019-08-16 22:07:23.713815302 -0400
+++ manifests/base-net-sw.yml   2019-08-16 18:00:32.000000000 -0400
@@ -31,7 +31,7 @@
         features:
         - mellanox-nic
         arch_versions:
-          all: 2.2.0
+          all: 2.245.0
   - group_name: gobgp
     submitter: Eran.Gampel@ibm.com
     maintainer: Eran.Gampel@ibm.com

#build the new payload
> make

*** Building payloads for hostos-upgrade ***
rm -rf ./output
mkdir -p ./output ./localrepo
cp -r ./manifests ./output/
docker build --no-cache -f Dockerfile -t payload_creator:latest .
Sending build context to Docker daemon  4.813MB
Step 1/7 : FROM wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/genctl-devtools2:latest
 ---> dcbdf6bd475b
Step 2/7 : ADD ./manifests/ /tmp/manifests
 ---> c595b34e3110
Step 3/7 : ADD ./payload_creator.py /tmp
 ---> 7ef4bc84129b
Step 4/7 : ADD ./hostos-common-tools/python/hostos_common/ /tmp/hostos_common
 ---> 1f9209315edb
Step 5/7 : ADD ./localrepo/ /tmp/localrepo
 ---> a09c3164e3f1
Step 6/7 : ADD ./build-project.sh /tmp
 ---> 3a53ab3bb293
Step 7/7 : ENTRYPOINT /tmp/build-project.sh
 ---> Running in f2c0619138c3
Removing intermediate container f2c0619138c3
 ---> 920dd235d4c9
Successfully built 920dd235d4c9
Successfully tagged payload_creator:latest

********

./zfsutils-linux_0.7.8-ibm2_amd64.deb
dpkg-scanpackages: warning: Packages in archive but missing from override file:
dpkg-scanpackages: warning:   cld-blockagent-cli cld-blockagent-server cld-computeproxy cld-computetools cld-nfsimageservice-cli cld-nfsimageservice-server cld-poolagent-cli cld-threadbare cni-plugins
dpkg-scanpackages: info: Wrote 9 entries to output Packages file.
cat: conf/distributions: No such file or directory
./
./InRelease
./Packages
./Packages.gz
./Release
./cld-blockagent-cli_1.0-1~20190730.1608_amd64.deb
./cld-blockagent-server_1.0-1~20190730.1608_amd64.deb
./cld-computeproxy_1.1.1~20190731.1721_amd64.deb
./cld-computetools_1.0-1~20190731.1715_amd64.deb
./cld-nfsimageservice-cli_1.0-1~20190730.1609~a61f1831f67922dcde1635c05803fc0e022c2464_amd64.deb
./cld-nfsimageservice-server_1.0-1~20190730.1609~a61f1831f67922dcde1635c05803fc0e022c2464_amd64.deb
./cld-poolagent-cli_1.0-1~20190730.1609~a61f1831f67922dcde1635c05803fc0e022c2464_amd64.deb
./cld-threadbare_1.0~20190815.1704_amd64.deb
./cni-plugins_1.0-1~20190502.1508_amd64.deb
./nextgen-os-sw_0.0.3-96b88d9_amd64.yml
rm -rf ./output/manifests
```

- Note:
This is the output of the above build:
version is 0.0.3-96b88d9, where 96b88d9 is the git has that is HEAD when
this build was kicked off.  Be if you don't make any commits prior to making
a local build, you may be creating a payload with the same version as a travis
built payload in artifactory.

```bash
[aross@alans-mbp.usma.ibm.com]/Users/aross/hostos/upgrade-payloads/
> ll output/
total 361M
-rw-r--r-- 1 aross staff 166M Aug 16 22:13 base-net-sw_0.0.3-96b88d9_amd64.tgz
-rw-r--r-- 1 aross staff 4.0K Aug 16 22:13 base-net-sw_0.0.3-96b88d9_amd64.yml
-rw-r--r-- 1 aross staff 171M Aug 16 22:14 base-os-sw_0.0.3-96b88d9_amd64.tgz
-rw-r--r-- 1 aross staff  25K Aug 16 22:14 base-os-sw_0.0.3-96b88d9_amd64.yml
-rw-r--r-- 1 aross staff  25M Aug 16 22:14 nextgen-os-sw_0.0.3-96b88d9_amd64.tgz
-rw-r--r-- 1 aross staff 3.0K Aug 16 22:14 nextgen-os-sw_0.0.3-96b88d9_amd64.ym

```


### Copy payload to release repo and make release bundle

* Here we copy the upgrade\-payload build output to the 'localrepo' folder in the base\-net\-sw\-release repo.
* Update the release bundle manifest to point to the local payload and 'make local'



**Build Release Bundle with Local Payload** 



``` bash
#copy new payload to 'localrepo' of release bundle repo
> cd ../base-net-sw-release/
> cp ../upgrade-payloads/output/* localrepo/

#update release bundle manifest to use this payload
> git diff
--- /var/folders/3r/b9s5n1wn3y92ynzzzy9p9sq40000gn/T//KC8QYb_hostos-base-net-sw-release.yml     2019-08-16 22:22:49.212705505 -0400
+++ hostos-base-net-sw-release.yml      2019-08-16 22:22:46.000000000 -0400
@@ -3,7 +3,7 @@
   release_version: 0.0.3
   payloads:
   - payload_type: base-net-sw
-    payload_version: 0.0.3-eb2e7c6
+    payload_version: 0.0.3-96b88d9
   tools:
   - tool_name: hostos-upgrade-tools
     tool_version: 0.0.3-6bc97fe

#build local release bundle
> make local
rm -rf ./output
mkdir -p ./output
cp -f ./localrepo/* ./output/
*** Building release bundle for hostos-base-net-sw-release ***
cp -r ./hostos-base-net-sw-release.yml ./output/hostos-base-net-sw-release_0.0.3-23940a5_all.yml
sed -i -e 's|release_version: 0.0.3|release_version: 0.0.3-23940a5|' ./output/hostos-base-net-sw-release_0.0.3-23940a5_all.yml
docker build -t hostos-base-net-sw-release:0.0.3-23940a5 --build-arg TOOLS_DOCKER_IMAGE=wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-upgrade-tools:0.0.3-6bc97fe --build-arg RELEASE_VERSION=0.0.3-23940a5 --build-arg PAYLOAD_VERSION=0.0.3-96b88d9 --build-arg TOOL_VERSION=0.0.3-6bc97fe -f /Volumes/gitvolume/hostos/base-net-sw-release//Dockerfile .
Sending build context to Docker daemon  1.337GB
Step 1/10 : ARG TOOLS_DOCKER_IMAGE=hostos-upgrade-tools:latest
Step 2/10 : FROM $TOOLS_DOCKER_IMAGE
 ---> 60f0439d130f
Step 3/10 : ARG RELEASE_VERSION
 ---> Using cache
 ---> 75c5ea6079fc
Step 4/10 : ARG PAYLOAD_VERSION
 ---> Using cache
 ---> 9e567a5e2849
Step 5/10 : ARG TOOL_VERSION
 ---> Using cache
 ---> d3bf2d766a9f
Step 6/10 : LABEL release_version="$RELEASE_VERSION"
 ---> Running in 2c4d222e6134
Removing intermediate container 2c4d222e6134
 ---> 0febdfeb4ae2
Step 7/10 : LABEL payload_version="$PAYLOAD_VERSION"
 ---> Running in f55c01ba695f
Removing intermediate container f55c01ba695f
 ---> 7dcbd71af9d3
Step 8/10 : LABEL tool_version="$TOOL_VERSION"
 ---> Running in e3249ba10781
Removing intermediate container e3249ba10781
 ---> c0cf53a537e0
Step 9/10 : COPY output/ /var/lib/nextgen/payloads/
 ---> 55374874a72e
Step 10/10 : CMD ["/bin/bash"]
 ---> Running in 42a1510eceea
Removing intermediate container 42a1510eceea
 ---> 2269642ead2b
Successfully built 2269642ead2b
Successfully tagged hostos-base-net-sw-release:0.0.3-23940a5

#see of this image is in local docker reg
> docker image ls | grep 0.0.3-23940a5
hostos-base-net-sw-release                                                                    0.0.3-23940a5       2269642ead2b        About a minute ago   575MB
```


### Import Local Release Bundle to Deploy Server

* Here we have to save the release bundle image to a local file, copy to the deploy server, and load the release bundle image into the local docker registry



**Import Release Bundle to Deploy Sever** 



```bash
# If others may use this release bundle per hostos operator notes, it's polite
# to add the standard artifactory path to the release bundle image.
# Though that makes it more easily confused with official release bundles from artifactory.

> docker tag hostos-base-net-sw-release:0.0.3-23940a5   wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-base-net-sw-release:0.0.3-23940a5
> docker image ls | grep 0.0.3-23940a5
hostos-base-net-sw-release                                                                     0.0.3-23940a5       2269642ead2b        8 minutes ago       575MB
wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-base-net-sw-release   0.0.3-23940a5       2269642ead2b        8 minutes ago       575MB

#save this image to local file
> docker save -o hostos-base-net-sw-release_0.0.3-23940a5 wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-base-net-sw-release:0.0.3-23940a5

#scp to deploy svr
> scp hostos-base-net-sw-release_0.0.3-23940a5 pok-deploy:
hostos-base-net-sw-release_0.0.3-23940a5                 100%  554MB   1.3MB/s   07:05

#docker load the image on the deploy svr
> docker load -i hostos-base-net-sw-release_0.0.3-23940a5
Loaded image: wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-base-net-sw-release:0.0.3-23940a5

> docker image ls | grep 0.0.3-23940a5
hostos-base-net-sw-release                                                                     0.0.3-23940a5       2269642ead2b        24 minutes ago      575MB
wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-base-net-sw-release   0.0.3-23940a5       2269642ead2b        24 minutes ago      575MB
```


### Run this bundle as upgrade

* This is the same as the hostos operator notes, just skipping the docker pull step for this bundle and use the tag 0\.0\.3\-23940a5



**Run new release bundle** Expand source



```bash
aross@pok1-qz1-sr1-rk000-s01:~$ ${HOSTOS_DOCKER//MZNAME/$MZONE}/hostos-base-net-sw-release:0.0.3-23940a5 nextgen-hostos-upgrade -y
Running nextgen-hostos-upgrade on release hostos-base-net-sw-release_0.0.3-23940a5 using payload base-net-sw_0.0.3-96b88d9 and hostos-upgrade-tools_0.0.3-6bc97fe ...
[mzone7470] [upgrade_nodes] ....................
  [edge pok1-qz1-sr1-rk001-s22] [upgrade_node] ....................
    [edge pok1-qz1-sr1-rk001-s22] [verify_current_configuration] ....................
    [edge pok1-qz1-sr1-rk001-s22] [verify_current_configuration] SUCCESS
    [edge pok1-qz1-sr1-rk001-s22] [determine_package_differences] ....................
    [edge pok1-qz1-sr1-rk001-s22] [determine_package_differences] SUCCESS
    [edge pok1-qz1-sr1-rk001-s22] [verify_not_too_disruptive] ....................
    [edge pok1-qz1-sr1-rk001-s22] [verify_not_too_disruptive] SUCCESS
    [edge pok1-qz1-sr1-rk001-s22] [transfer_payloads] ....................
    [edge pok1-qz1-sr1-rk001-s22] [transfer_payloads] SUCCESS
    [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run] ....................
      [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run_mlxcfg] ....................
      [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run_mlxcfg] SUCCESS
      [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run_gobgp] ....................
      [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run_gobgp] SUCCESS
      [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run_cloudnet] ....................
      [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run_cloudnet] SUCCESS
    [edge pok1-qz1-sr1-rk001-s22] [install_packages_dry_run] SUCCESS (Ok: 3 Failed: 0)
    [edge pok1-qz1-sr1-rk001-s22] [install_packages] ....................
      [edge pok1-qz1-sr1-rk001-s22] [install_packages_mlxcfg] ....................

****

    [compute pok1-qz1-sr1-rk002-s22] [install_packages_dry_run] SUCCESS (Ok: 2 Failed: 0)
    [compute pok1-qz1-sr1-rk002-s22] [install_packages] ....................
      [compute pok1-qz1-sr1-rk002-s22] [install_packages_mlxcfg] ....................
      [compute pok1-qz1-sr1-rk002-s22] [install_packages_mlxcfg] SUCCESS
      [compute pok1-qz1-sr1-rk002-s22] [install_packages_cloudnet] ....................
      [compute pok1-qz1-sr1-rk002-s22] [install_packages_cloudnet] SUCCESS
    [compute pok1-qz1-sr1-rk002-s22] [install_packages] SUCCESS (Ok: 2 Failed: 0)
    [compute pok1-qz1-sr1-rk002-s22] [verify_packages_installed] ....................
    [compute pok1-qz1-sr1-rk002-s22] [verify_packages_installed] SUCCESS
    [compute pok1-qz1-sr1-rk002-s22] [mark_stage_complete] ....................
    [compute pok1-qz1-sr1-rk002-s22] [mark_stage_complete] SUCCESS
    [compute pok1-qz1-sr1-rk002-s22] [cleanup_repositories] ....................
  [compute pok1-qz1-sr1-rk002-s22] [upgrade_node] SUCCESS  [3 min 59.57 sec]
[mzone7470] [upgrade_nodes] SUCCESS (Ok: 2 Failed: 0)  [8 min 13.49 sec]
aross@pok1-qz1-sr1-rk000-s01:~$
```
