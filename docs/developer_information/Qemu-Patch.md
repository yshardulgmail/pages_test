---
layout: default
title:  Qemu Patch
parent: Developer Information
---

## How to Apply Patches to ibmcloud qemu Ubuntu 22.04

This page describes how we should build ibmcloud qemu from Canonical. If
you\'re reading this, chances are you want to add some custom patches to
qemu and build it for testing.

### Using pull-ppa-source

You need to know your ppa location, in this case we\'re using
\--ppa=ibm-cloud/sf-345863-jammy-virt-stack\
\
- Let\'s pull the source\
% cd /data/qemu-build;  pull-ppa-source -L \--distro=ppa
\--ppa=ibm-cloud/sf-345863-jammy-virt-stack qemu\
\
pulls in these files into your :

qemu-6.2+dfsg/   \<\-\-- this is your patched source directory.\
qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3.debian.tar.xz  \<\-\-- This
contains debian rules, changelog and ibmcloud specific patches\
qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3.dsc   \<\-\-- This is the build
recipe file\
qemu_6.2+dfsg.orig.tar.xz \<\-\-- this is the base qemu source

Notes: if you\'re unable to pull. Be sure to setup the credential file
once:

-   export LP_CREDENTIALS_FILE=\~/.lpcred

-   pull-ppa-source -L \--distro=ppa \--ppa=ibm-cloud/release libvirt
    jammy \### on first run, you may get something like this:

-   root@[2204builder1:/data2#](http://2204builder1/data2)
    pull-ppa-source -L  \--distro=ppa \--ppa=ppa:ibm-cloud/release
    libvirt jammy\
    The authorization page:\
     (<https://launchpad.net/+authorize-token?oauth_token=30lwKFxQBfsGT5V0tghg&allow_permission=DESKTOP_INTEGRATION>)\
    should be opening in your browser. Use your browser to authorize\
    this program to access Launchpad on your behalf.\
    Waiting to hear from Launchpad about your decision\...

-   copy and paste the https:// link above into a web browser, authorize
    the access. Once completed, come back to your terminal and hit enter
    to continue.

\*\* You might need to disable lynx, if you see that text-based web
browser trying to load. Try renaming /usr/bin/lynx to
/usr/bin/lynx-no-use.

**Adding a patch to the build**

- in your patched source, qemu-6.2+dfsg/ set up a local git tree.\
% git init; git add \*; git commit -m \"initial commit

\- put in your changes in the git tree using \"git am your.patch\" or
\"patch \< your.patch\" or manually put in the changes. Then git add and
commit your changes.

\- regenerate your patch for qemu build with \"git format-patch -1\" to
get the last commit. (Use -X to get the last X commits.)

\- put your patch in the ibmcloud tarball\
% mkdir /tmp/qemu-tmp; cp
qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3.debian.tar.xz /tmp/qemu-tmp/;
unxz qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3.debian.tar.xz; tar xvf
qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3.debian.tar\
This creates a \"debian\" directory. Put your patch in
debian/patches/ibm/ directory and update \"debian/patches/series\" to
list your patch. (Note that debian/patches/ibm/series didn\'t seem to do
anything, so no need to update this file.)\
Update debian/changelog, give it a new version, ie: add \"test1\" to the
version string. Also, summarize your changes there.\
Create a patched tarball, marking it as \"test1\" here. This test1
should match the changelog.\
% tar cvf qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3test1.debian.tar
debian; xz qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3test1.debian.tar.xz

\- rename your recipe file to include test1 in its name\
% cd /data/qemu-build; mv qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3.dsc
qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3test1.dsc

\- update recipe file to reflect the new version, sharsum, sha256sum,
md5sum, and size of
 qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3test1.debian.tar.xz file

**Build it!**

\- If you don\'t have a chroot yet, create one with:\
% sudo sbuild-launchpad-chroot create -n jammy-proposed-amd64 -s jammy
-a amd64\
if you want to update an exising chroot\
% sudo sbuild-update -udcar jammy-proposed-amd64\
- Ok! now build it\
% sudo sbuild -Adjammy-proposed-amd64
qemu_6.2+dfsg-2ubuntu6.5\~ibmcloud0.0.3test1.dsc

## **Another way to pull the source\...**

1. Git checkout virt-stack-ibmcloud repository, it has IBM specific
    changes:

    1. git clone
        git+[ssh://\<your-username\>@git.launchpad.net/\~ibm-cloud/ibm-cloud-private/+git/virt-stack-ibmcloud](ssh://danymadden@git.launchpad.net/~ibm-cloud/ibm-cloud-private/+git/virt-stack-ibmcloud)\
        \# Hopefully you know which branch you want to use. If not,
        maybe use HEAD. Otherwise, ask Canonical.

    2. cd virt-stack-ibmcloud; git checkout -b
        SF-345863-jammy-virt-stack-v2
        origin/SF-345863-jammy-virt-stack-v2

    3. if you have additional patches to add for testing, then put your
        patches in virt-stack-ibmcloud/patches.qemu/ibm directory. And
        then, append the patch names to the end of the \"ibmseries\"
        file. \*\*\*This assumes that you have sanitized your patches
        and made sure that they will apply cleanly.\
        \
        If patches have conflicts, then continue to step 2 and 3 below
        to get a patched source to base your changes on.

2. Apply this patch to virt-stack-ibmcloud/update-ibmcloud.sh to skip
    signing.

            **unsigned build**

            ```bash

            diff \--git a/update-ibmcloud.sh b/update-ibmcloud.sh

            index b4d473e..cafbc21 100755

            \-\-- a/update-ibmcloud.sh

            +++ b/update-ibmcloud.sh

            @@ -259,7 +259,7 @@ build () {

                 fi

                 echo \"IBM: Source Build\"

            \-    if ! dpkg-buildpackage -S -nc -d -sa -v\"\${oldversion}\"; then

            \+    if ! dpkg-buildpackage -S -nc -d -uc -us -v\"\${oldversion}\";
            then

                     echo \"Errors during source build, please check content in
            \${wdir}\"

                     echo \"Most likely the patches in need to be adapted\"

                 else
            ```
            Check to see if you have \"git ubuntu\" installed. If not install it
            with:\
            a. snap install git-ubuntu \--classic\
            b. snap switch --edge git-ubuntu\
            c. snap refresh\
            \* b and c may not be needed if latest/stable is at least
            1.0-124-g5c99643.

3. Build source with update-ibmcloud.sh. This will apply all patches
    from the qemu main repo and patches from virt-stack-ibmcloud that
    you checkout above.

    1. export DEBFULLNAME=\"Your Name\"; export
        DEBEMAIL=\"your_email@[us.ibm.com](http://us.ibm.com)\"; export
        GPGKEY=\"JUNKSTRING-we-are-not-signing\";

    2. ./update-ibmcloud.sh build qemu jammy none notag. (later we
        found that for libvirt, we need \"jammy\" : ./update-ibmcloud.sh
        build libvirt jammy none notag)

    3. take note of where your qemu\*dsc file is. You\'ll need it for
        step 5.

    4. You many not need to do this step. \*\*This is continuing from
        step 1c, where you\'re needing a patched source to base your
        changes on to resolve your patch conflicts.

        1. setup quilt. create this \~/.quiltrc file with the following
            content:

                **.quiltrc**

                ```bash
                QUILT_PATCHES=debian/patches

                QUILT_NO_DIFF_INDEX=1

                QUILT_NO_DIFF_TIMESTAMPS=1

                QUILT_REFRESH_ARGS=\"-p ab\"

                QUILT_DIFF_ARGS=\"\--color=auto\" \# If you want some color when using
                \`quilt diff\`.

                QUILT_PATCH_OPTS=\"\--reject-format=unified\"

                QUILT_COLORS=\"diff_hdr=1;32:diff_add=1;34:diff_rem=1;31:diff_hunk=1;33:diff_ctx=35:diff_cctx=33\"
                ```

        2. update-ibmcloud.sh created /tmp/tmp.ibmcloud.qemu.\<unique\>/qemu
        directory. You will build your patched source here. cd
        /tmp/tmp.ibmcloud.qemu.\<unique\>/qemu; 

        3. quilt push -a; \# this applies all patches in debian/patches to give
        you your patched source! Keep working in here until you have all of
        your patches, then to back to step 1c.

4. Create launchpad chroot environment to build

    1. sudo sbuild-launchpad-chroot create -n jammy-proposed-amd64 -s
        jammy -a amd64

    2. sudo sbuild-update -udcar jammy-proposed-amd64

5. build it!

    1. cd /tmp/tmp.ibmcloud.qemu.\<unique\>/; sbuild
        -Adjammy-proposed-amd64 qemu\*\~ibmcloud\*.dsc
