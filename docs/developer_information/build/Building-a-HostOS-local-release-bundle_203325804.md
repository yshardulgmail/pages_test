---
layout: default
title: Local Release Bundle 
parent: Build
grand_parent: Developer Information
---


# Building a HostOS local release bundle

Building a local base-os-sw,base-net-sw or nextgen-os-sw release bundle

Following steps will provide the steps to build a local release bundle for
base-os-sw.Similar steps are applicable for other bundles listed above.

1. Build pre-requisites : setting build environment variables

      ```bash
      set MY_TAG (optional) - you can set a custom tag to be suffixed to all artifacts generated after build. eg. 3.0.0-hp24032021
      set ARTIFACTORY_LOGIN=<your artifactory user_id>
      set ARTIFACTORY_ACCESS_TOKEN=<your artifactory api_key>
      set HOSTOS_DOCKER_REPO=[wcp-genctl-docker-local.artifactory.swg-devops.com/hostos](http://wcp-genctl-docker-local.artifactory.swg-devops.com/hostos)
      set HOSTOS_SANDBOX_REPO=[wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos](http://wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos)
      set GIT_PAT= <your github token> //(Not the artifactory token)
      ```

      Export

      ``` bash
      export MY_TAG=<custom_tag>
      export ARTIFACTORY_LOGIN=<y=user_id>
      export ARTIFACTORY_ACCESS_TOKEN=<token>
      export HOSTOS_DOCKER_REPO=wcp-genctl-docker-local.artifactory.swg-devops.com/hostos
      export HOSTOS_SANDBOX_REPO=wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos
      export GIT_PAT=<github-token>
      ```
  
2. Build a local payload

      - checkout the payload repository from git https://github.ibm.com/cloudlab/hostos-upgrade-payloads
      - You may provide a **custom boot image or a set of custom debian packages** under the localrepo folder in the in root project directory.Custom debian packages if any will be automatically picked up.
      - Update the manifest yaml files under <PROJECT_DIR>/manifests accordingly to reflect the new debian package versions to be picked.
      - If you are prompted for user name and password please ensure you give your github token as the password.
      - Build the hostos-upgrade-payloads using
      a. Normal Build with or without custom debian packages in locarepo

        ```bash
          make GITHASH=$MY_TAG
        ```

        b. Build with custom boot image

        ```bash
          make USELOCAL=1 GITHASH=$MY_TAG
        ```

        - The payloads tar and other manifests will be generated under the output folder
        - If you run into errors for missing or invalid versions of dependency packages, you may generate the appropriate dependency yaml manifest to include required packages using **make VALIDATE=0**
        - Add/Update this dependency yaml file under the manifest folder at - <PROJECT_DIR>/manifests and rebuild using make

3. Build a local tool image ( Most of the time tool needn't be built )

      - Checkout the tools repository from git  https://github.ibm.com/cloudlab/hostos-upgrade-tools
      - Build the hostos-upgrade-tools using
      - Build tools

      ```bash
         make GITVERSION=$MY_TAG build
      ```
      - Build tools docker image
      ```bash 
      make GITVERSION=$MY_TAG docker
       ```
      - Tag docker image to be consumed by local release bundle build
      
      ```bash
      docker image tag hostos-upgrade-tools:$MY_TAG $HOSTOS_SANDBOX_REPO/hostos-validate-tools:$MY_TAG
      
      ```

4. Build a local release bundle

    Recently the procedure to create a local hostos build has changed
    significantly. The main reason for the change:

    - Earlier artifactory allowed direct read access but this is not compliant to the security standards. So, it blind-read-access is enforced.
    - During one pipeline, HostOS was carrying PCE to fix 18.04 based VULN in hostos release bundles, we ended up doing some changes ( like gpg key ) to make it work.

     A series of access need to be asked for in order to proceed:

    - Fyre VM needs to be requested for - It contains Ubuntu security GPG  
    [Requesting a VM using Fyre](Requesting-a-VM-using-Fyre_201457902.html)  

    - To configure VM to run our builds , run this shell script
    [config.sh](https://confluence.swg.usma.ibm.com:8445/download/attachments/157353569/config.sh?version=1&modificationDate=1659407338739&api=v2)
    from the local host.

    - Once you get access to Fyre VM, access it either using Cisco anyconnect VPN or
    from the pok machine itself.

    - [Request access to Artifactory repositories](https://confluence.swg.usma.ibm.com:8445/display/DevOps/Request+access+to+Artifactory+repositories)
    - [Request access to HostOS Artifactory repositories](https://confluence.swg.usma.ibm.com:8445/display/HCP/Request+access+to+HostOS+Artifactory+repositories)
    - Checkout the release bundle repository from git - **[https://github.ibm.com/cloudlab/hostos-base-os-sw-releas](https://github.ibm.com/cloudlab/hostos-base-os-sw-release)**

    - You may chose to build a release bundle with the existing payload and tool artifacts picked from artifactory or supply the artifacts built by your local builds
    - To use the locally build payload artifacts, copy the contents from the output folder of hostos-upgrade-payloads to localrepo folder of hostos-base-os-sw-release
    - Create the _localrepo_ folder if it dosen't exist.
    - Update the hostos-base-os-sw-release.yml to reflect correct versions of payload and tools atrtifacts
    - Build the release bundle docker image using 
    - set CC_ARTIF_ACCESS_TOKEN to your artifactory token as the makefile expects value from CC_ARTIF_ACCESS_TOKEN env variable
    - export CC_ARTIF_ACCESS_TOKEN=$ARTIFACTORY_ACCESS_TOKEN
    - Build with localrepo

      ```bash
          make GITVERSION=$MY_TAG local
      ```

    - Normal build

      ```bash
          make GITVERSION=$MY_TAG
      ```

    - If you plan to deploy the release bundle via DDT, then you need to tag the release bundle image as follows:

     ```bash
      docker image tag hostos-base-os-sw-release:$MY_TAG $HOSTOS_DOCKER_REPO/hostos-base-os-sw-release:$MY_TAG
     ```

    - Your local release bundle is now ready
    - If you need to copy your release bundle image from your build/local machine to a remote deployer , then save it using :
  
    ```bash
    docker save $HOSTOS_DOCKER_REPO/hostos-base-os-sw-release:$MY_TAG | gzip > hostos-base-os-sw-release-$MY_TAG.tgz
     ```

    - Copy tar to deployer using scp or other methods

    - If you are using tsh client to copy to shellng server then use :

    - tsh scp hostos-base-os-sw-release-$MY_TAG.tgz <softlayer_user_id>@$SHELLNG:/home/<softlayer_user_id>/hostos-base-os-sw-release-$MY_TAG.tgz

    - load the docker image from the tar using

    ```bash
    docker load < hostos-base-os-sw-release-$MY_TAG.tgz
    ```
 
     - Pushing and pulling images to/from Artifactory

On the build server after building the release bundle image, we can push the
image to Artifactory. And in the deployer, we can download the locally built
image from Artifactory. Certain steps need to be followed for that.

1. Tag the image in the format: Art-URL/Repo/Image:tag  

      ```bash
      e.g. docker tag [docker.io/library/hostos-base-net-
      release:6.2.0-20231114T0](http://docker.io/library/hostos-pds-config-
      release:6.2.0-20231114T0) [docker-na-public.artifactory.swg-devops.com/wcp-
      genctl-sandbox-docker-local/hostos-](http://docker-na-public.artifactory.swg-
      devops.com/wcp-genctl-sandbox-docker-local/hostos-pds-config-
      release:6.2.0-20231114T0)[base-net](http://docker.io/library/hostos-pds-
      config-release:6.2.0-20231114T0)-[release:6.2.0-20231114T0](http://docker-na-
      public.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local/hostos-pds-
      config-release:6.2.0-20231114T0)
      ```

2. Push the image into Artifactory  

      ```bash
      docker push [docker-na-public.artifactory.swg-devops.com/wcp-genctl-sandbox-
      docker-local/hostos-](http://docker-na-public.artifactory.swg-devops.com/wcp-
      genctl-sandbox-docker-local/hostos-pds-config-release:6.2.0-20231114T0)[base-
      net](http://docker.io/library/hostos-pds-config-
      release:6.2.0-20231114T0)[-release:6.2.0-20231114T0](http://docker-na-
      public.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local/hostos-pds-
      config-release:6.2.0-20231114T0)
      ```

3. On the deployer, download the image. We need to login to docker registry for before downloading:  

    ```bash
      echo $ARTIF_APIKEY|docker login -u $ARTIF_USER [docker-na-
      private.artifactory.swg-devops.com](http://docker-na-private.artifactory.swg-
      devops.com/) \--password-stdin
    ```

4. Download the image:  

      ```bash
      docker pull [docker-na-private.artifactory.swg-devops.com/wcp-genctl-sandbox-
      docker-local/hostos-](http://docker-na-private.artifactory.swg-devops.com/wcp-
      genctl-sandbox-docker-local/hostos-pds-config-release:6.2.0-20231114T0)[base-
      net](http://docker.io/library/hostos-pds-config-
      release:6.2.0-20231114T0)[-release:6.2.0-20231114T0](http://docker-na-
      private.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local/hostos-pds-
      config-release:6.2.0-20231114T0)
      ```

5. Again tag it to the right name which can be consumed by ddt:  

      ```bash
      docker tag [docker-na-private.artifactory.swg-devops.com/wcp-genctl-sandbox-
      docker-local/hostos-](http://docker-na-private.artifactory.swg-devops.com/wcp-
      genctl-sandbox-docker-local/hostos-pds-config-release:6.2.0-20231114T0)[base-
      net](http://docker.io/library/hostos-pds-config-
      release:6.2.0-20231114T0)[-release:6.2.0-20231114T0](http://docker-na-
      private.artifactory.swg-devops.com/wcp-genctl-sandbox-docker-local/hostos-pds-
      config-release:6.2.0-20231114T0) [docker-na-private.artifactory.swg-
      devops.com/wcp-genctl-docker-local/hostos/hostos-](http://docker-na-
      private.artifactory.swg-devops.com/wcp-genctl-docker-local/hostos/hostos-pds-
      config-release:6.2.0-20231114T0)[base-net](http://docker.io/library/hostos-
      pds-config-release:6.2.0-20231114T0)[-release:6.2.0-20231114T0](http://docker-
      na-private.artifactory.swg-devops.com/wcp-genctl-docker-local/hostos/hostos-
      pds-config-release:6.2.0-20231114T0)
      ```

6. Make sure to add below tag in the hjson file for the nextgen package

      ```bash
      registry: local
      ```