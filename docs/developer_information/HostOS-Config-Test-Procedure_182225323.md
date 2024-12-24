---
layout: default
title: Config Test Procedure 
parent: Developer Information
---

## HostOS Config Test Procedure



1. **Does it work properly in a fresh deploy scenario**
* from the correct branch, make your fix in the hostos\-config\-payloads repo
* run: git commit, make clean \&\& make



**Make** 



```
sshelke@dal2-qz2-sr2-rk219-s03:~/2040/hostos-config-payloads$ make
*** Building payload for hostos-config ***
rm -rf ./output
mkdir -p ./output
cp -r ./hostos-config.yml ./scripts ./share ./resource   ./output/
git show f1a926a > ./output/git.f1a926a
sed -i -e 's|payload_version: 2.1.0|payload_version: 2.1.0-f1a926a|' ./output/hostos-config.yml
# Do a basic yaml processing to convert the Port Metadata YAML file to a JSON file for the config script to use
./convert-allowed-ports.sh ./hostos-allowed-ports.yml ./output/hostos-allowed-ports.json
tar -czf ./output/hostos-config_2.1.0-f1a926a_all.tgz -C ./output hostos-config.yml scripts share ./resource git.f1a926a hostos-allowed-ports.json 
cp ./output/hostos-config.yml ./output/hostos-config_2.1.0-f1a926a_all.yml
rm -rf ./output/scripts ./output/share ./output/hostos-config.yml
```
* get the short hash id from the commit log with:   
git log \-\-oneline \| head \-1 OR git log \-\-pretty\=format:"%h \- %an, %ar : %s”" \| head \-1



**Git log** 



```
sshelke@dal2-qz2-sr2-rk219-s03:~/2040/hostos-config-payloads$ git log --oneline | head -1
f1a926a added sysdig-agent apparmor profile
```
* note the payload\_version (x.x.x) in the hostos\-config.yml (this version is tied to the branch that you are working from)
* From the correct branch in the `hostos-config-release` repo, edit the `hostos-config-release.yml`and set payload version value to be:  
 `payload_version: <x.x.x>-<the_short_hash_of_payload>`
* `and update release version value to be` incremental value of existing value
* `build the hostos-config-release bundle with "make local"`



**Make Local** 



```
mkdir hostos-config-release/localrepo 
cp hostos-config-payloads/output/hostos-config* hostos-config-release/localrepo/ 

sshelke@dal2-qz2-sr2-rk219-s03:~/2040/hostos-config-release$ make local
rm -rf ./output
mkdir -p ./output
cp -f ./localrepo/* ./output/
*** Building release bundle for hostos-config-release ***
cp -r ./hostos-config-release.yml ./output/hostos-config-release_2.4.13-20210329T081001Z_3527309_all.yml
sed -i -e 's|release_version: 2.4.13|release_version: 2.4.13-20210329T081001Z_3527309|' ./output/hostos-config-release_2.4.13-20210329T081001Z_3527309_all.yml
docker build  -t hostos-config-release:2.4.13-20210329T081001Z_3527309 --build-arg TOOLS_DOCKER_IMAGE=wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-config-tools:2.0.0-4f28611 --build-arg RELEASE_VERSION=2.4.13-20210329T081001Z_3527309 --build-arg PAYLOAD_VERSION=2.1.0-f1a926a --build-arg TOOL_VERSION=2.0.0-4f28611 -f /home/sshelke/2040/hostos-config-release//Dockerfile .
Sending build context to Docker daemon  519.7kB
Step 1/10 : ARG TOOLS_DOCKER_IMAGE=hostos-config-tools:latest
Step 2/10 : FROM $TOOLS_DOCKER_IMAGE
 ---> 96c4405afe1c
Step 3/10 : ARG RELEASE_VERSION
 ---> Using cache
 ---> 603428f8fd2b
Step 4/10 : ARG PAYLOAD_VERSION
 ---> Using cache
 ---> 4a65f49d8634
Step 5/10 : ARG TOOL_VERSION
 ---> Using cache
 ---> 9ba47c518d93
Step 6/10 : LABEL release_version="$RELEASE_VERSION"
 ---> Running in 21703892f769
Removing intermediate container 21703892f769
 ---> 27375864ca77
Step 7/10 : LABEL payload_version="$PAYLOAD_VERSION"
 ---> Running in 7acda904f6e1
Removing intermediate container 7acda904f6e1
 ---> 0184a3959bab
Step 8/10 : LABEL tool_version="$TOOL_VERSION"
 ---> Running in 37c819b76fe8
Removing intermediate container 37c819b76fe8
 ---> f02147c475e0
Step 9/10 : COPY output/ /var/lib/nextgen/payloads/
 ---> 4acbeac8a88b
Step 10/10 : CMD ["/bin/bash"]
 ---> Running in df4f8fdbe00a
Removing intermediate container df4f8fdbe00a
 ---> 2cff8c6091b6
Successfully built 2cff8c6091b6
Successfully tagged hostos-config-release:2.4.13-20210329T081001Z_3527309
```
* run : docker save, docker load and docker tag



**Docker save, load and tag** 



```
sshelke@dal2-qz2-sr2-rk219-s03:~/2040/hostos-config-release$ docker save -o /home/sshelke/hostos-config-release-2.4.13-20210329T081001Z_3527309.tgz hostos-config-release:2.4.13-20210329T081001Z_3527309
sshelke@dal2-qz2-sr2-rk219-s03:~/2040/hostos-config-release$ docker load < /home/sshelke/hostos-config-release-2.4.13-20210329T081001Z_3527309.tgz 
sshelke@dal2-qz2-sr2-rk219-s03:~/2040/hostos-config-release$ docker images | head -5
REPOSITORY                                                                               TAG                                                         IMAGE ID            CREATED              SIZE
hostos-config-release                                                                    2.4.13-20210329T081001Z_3527309                             2cff8c6091b6        About a minute ago   343MB
wcp-genctl-docker-local.artifactory.swg-devops.com/rias/rias-release                     20210328T233132Z_599019849ea12748104b2e219c067f1bfe9b3a96   d7d1585dd5c3        9 hours ago          312MB
wcp-genctl-docker-local.artifactory.swg-devops.com/genctl/genctl-release                 20210328T232108Z_14714990cbca245a9c2e3294d3627eb583dca304   cbaa45b7b59e        9 hours ago          312MB
wcp-genctl-docker-local.artifactory.swg-devops.com/hostos/hostos-nextgen-os-sw-release   cee13c8baf865e2c2ffce1b00c26f319dc9b9948                    5a2f1bee0108        25 hours ago         313MB
sshelke@dal2-qz2-sr2-rk219-s03:~/2040/hostos-config-release$ docker tag 2cff8c6091b6 wcp-genctl-docker-local.artifactory.swg-devops.com/hostos/hostos-config-release:2.4.13-20210329T081001Z_3527309
```
* Deploy using ddtool: edit mzone\<xxxx\>.hjson file to update tag. (note registry: local for hosts\-config\-release)
* you can find vetted versions for other releases from here \-  [https://github.ibm.com/genctl\-cicd/vetted\-versions/blob/master/pre\-integration.yaml](https://github.ibm.com/genctl-cicd/vetted-versions/blob/master/pre-integration.yaml)
* **Update tag in mzonexxxx.hjson** Expand source



```
{
  components: {
    hostos: {
      packages: {
        hostos-boot-release: {
          forcereboot: yes
          tag:"2.2.0-20201204T032546Z_7752b7a"
          artifrepo: wcp-genctl-docker-local.artifactory.swg-devops.com
        }
        hostos-config-release: {
          tag: "2.4.13-20210329T081001Z_3527309"
          registry: local
#          artifrepo: wcp-genctl-docker-local.artifactory.swg-devops.com
        }
        hostos-base-os-sw-release: {
          tag: "2.3.1-20201215T201529Z_84bafb6"
          artifrepo: wcp-genctl-docker-local.artifactory.swg-devops.com
        }
        hostos-base-net-sw-release: {
          tag: "2.1.7-20201217T033611Z_c52d1e1"
          artifrepo: wcp-genctl-docker-local.artifactory.swg-devops.com
        }
        hostos-nextgen-os-sw-release: {
          tag: "2.1.6-20201217T010522Z_5f47b6c"
          artifrepo: wcp-genctl-docker-local.artifactory.swg-devops.com
        }
        hostos-post-config-release: {
          tag: "2.4.13-20210329T081001Z_3527309"
          registry: local
#          artifrepo: wcp-genctl-docker-local.artifactory.swg-devops.com
        }
      }
    }

    kube: {
      packages: {
        etcd-base-release: {
          tag: "2.0.3_20201215T215819Z_6f3e7ee"
        }
        kube-base-release: {
          tag: "2.0.6_20201207T180300Z_4fff6d7"
        }
        kube-define-release: {
          tag: "2.0.4_20201215T222928Z_625389e"
        }
        kube-addon-release: {
          tag: "2.0.3_20201215T222948Z_ea2d4ec"
        }
      }
    }
    genctl: {
      packages: {
        genctl-release: {
          tag: "20201217T125950Z_a4cdcf87ab108a138579df76962f2a7c5c09d1e1"
        }
        genctl-telemetry-release: {
          tag: "20201214T200046Z_b6eae6ae271a979790b362da029af1cfb293ce57"
        }
      }
    }
  }
  mzone: mzone7204
}


```
* deploy using ddtool: ./ddt.sh \-i\=mzone\<xxxx\>.hjson \-\-summary
* Once deployment is successful, login to any node and verify by *'cat /etc/genesis/release\_bundles'*
* Verify if the fix is delivered and working as expected.
* Grab a snip for evidence.

**2\. Does it work properly in an upgrade scenario**

* Follow above steps till editing mzone.\<xxxx\>.hjson and now only edit tag for hosts\-config\-release to newly created TAG
* deploy using ddtool: ./ddt.sh \-i\=mzone\<xxxx\>.hjson \-\-summary
* Once deployment is successful, login to any node and verify by *'cat /etc/genesis/release\_bundles'*
* Verify if the fix is delivered and working as expected in upgrade scenario.
* Grab a snip for evidence.

**3\.  Does it work properly the next time the script is run after it has been applied once**

get rid of the checksum file (from below) that you want to make sure that they need to run again after an upgrade. The behavior should be that the checksum will fail without the file and the verify will force the config to perform/execute.



**Checksum** Expand source



```
ls -lart /etc/genesis/*.md5sum
-rw-r--r-- 1 root root  132 Sep 17  2020 /etc/genesis/eventing.md5sum
-rw-r--r-- 1 root root  127 Sep 17  2020 /etc/genesis/flowlogs.md5sum
-rw-r--r-- 1 root root  398 Sep 17  2020 /etc/genesis/kdump.md5sum
-rw-r--r-- 1 root root  130 Sep 17  2020 /etc/genesis/storage.md5sum
-rw-r--r-- 1 root root  121 Sep 17  2020 /etc/genesis/nessus.md5sum
-rw-r--r-- 1 root root  132 Sep 17  2020 /etc/genesis/bigfix.md5sum
-rw-r--r-- 1 root root  188 Sep 17  2020 /etc/genesis/ntp.md5sum
-rw-r----- 1 root root  133 Oct  2 00:39 /etc/genesis/falconsensor.md5sum
-rw-r--r-- 1 root root  130 Nov  9 21:12 /etc/genesis/apparmor.md5sum
-rw-r--r-- 1 root root  342 Nov 18 23:22 /etc/genesis/docker.md5sum
-rw-r--r-- 1 root root  128 Feb  1 17:08 /etc/genesis/secure.md5sum
-rw-r----- 1 root root  115 Feb  5 22:39 /etc/genesis/krb.md5sum
-rw-r--r-- 1 root root   57 Feb  8 20:05 /etc/genesis/users.md5sum
-rw-r--r-- 1 root root  122 Feb  8 21:27 /etc/genesis/sshd.md5sum
-rw-r----- 1 root root   58 Feb 11 13:13 /etc/genesis/master.md5sum
-rw-r----- 1 root root   62 Feb 13 15:11 /etc/genesis/tlslibvirt.md5sum
-rw-r----- 1 root root   58 Feb 13 19:54 /etc/genesis/vagent.md5sum
-rw-r----- 1 root root   63 Feb 16 20:14 /etc/genesis/pamapparmor.md5sum
-rw-r--r-- 1 root root  474 Mar  9 19:36 /etc/genesis/specialservices.md5sum
-rw-r--r-- 1 root root   59 Mar 23 14:32 /etc/genesis/network.md5sum
-rw-r--r-- 1 root root 6138 Mar 23 14:32 /etc/genesis/common.md5sum
-rw-r--r-- 1 root root  747 Mar 26 01:33 /etc/genesis/coreinfo.md5sum
-rw-r----- 1 root root  185 Mar 26 16:50 /etc/genesis/ipset.md5sum
```




