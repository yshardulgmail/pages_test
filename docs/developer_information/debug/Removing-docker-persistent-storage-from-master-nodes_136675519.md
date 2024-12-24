---
layout: default
title: Removing docker persistent storage from master nodes 
parent: Debug
grand_parent: Developer Information
---
#### On master node:

You can see the persistent storage on the master node by running the df command:

root@aus1\-qz2\-rm1\-rk018\-s35:\~\# df  
Filesystem                      1K\-blocks    Used  Available Use% Mounted on  
/dev                            261030596       0  261030596   0% /dev  
rootfs                           20971520 3208080   17763440  16% /  
tmpfs                           261684704      36  261684668   1% /dev/shm  
tmpfs                           261684704    4904  261679800   1% /run  
tmpfs                                5120       0       5120   0% /run/lock  
tmpfs                           261684704       0  261684704   0% /sys/fs/cgroup  
tmpfs                           261684704     120  261684584   1% /tmp  
tmpfs                           261684704       0  261684704   0% /var/tmp  
/dev/mapper/disk1\-varlibetcd   1055840692  471148 1001666072   1% /var/lib/etcd  
/dev/mapper/disk1\-varlibdocker 1055840692 1675068 1000462152   1% /var/lib/docker  
tmpfs                            52336940       0   52336940   0% /run/user/0

Note that both etcd and docker have persistent storage defined; we will be removing the docker storage.

On the master node you can view the persistent storage, defined via the lvdisplay command:

root@aus1\-qz2\-rm1\-rk018\-s35:\~\# **lvdisplay**  
  \-\-\- Logical volume \-\-\-  
  LV Path                /dev/disk1/varlibetcd  
  LV Name                varlibetcd  
  VG Name                disk1  
  LV UUID                fD0AYk\-twS1\-ACZ3\-3VP4\-4ryD\-6KfQ\-ggNQCn  
  LV Write Access        read/write  
  LV Creation host, time aus1\-qz2\-rm1\-rk018\-s35, 2020\-03\-11 18:28:40 \+0000  
  LV Status              available  
  \# open                 1  
  LV Size                1\.00 TiB  
  Current LE             262144  
  Segments               1  
  Allocation             inherit  
  Read ahead sectors     auto  
  \- currently set to     256  
  Block device           253:0  
    
  \-\-\- Logical volume \-\-\-  
  LV Path                /dev/disk1/varlibdocker  
  LV Name                varlibdocker  
  VG Name                disk1  
  LV UUID                YWpJ21\-qbcM\-nQcx\-8rly\-Vvns\-IdKw\-rCXFnH  
  LV Write Access        read/write  
  LV Creation host, time aus1\-qz2\-rm1\-rk018\-s35, 2020\-07\-15 15:18:36 \+0000  
  LV Status              available  
  \# open                 1  
  LV Size                1\.00 TiB  
  Current LE             262144

  Segments               1  
  Allocation             inherit  
  Read ahead sectors     auto  
  \- currently set to     256  
  Block device           253:1

The pvdisplay command shows the physical volumes associated with the logical volumes:

root@aus1\-qz2\-rm1\-rk018\-s35:\~\# **pvdisplay**  
  \-\-\- Physical volume \-\-\-  
  PV Name               /dev/sda  
  VG Name               disk1  
  PV Size               \<10\.48 TiB / not usable 4\.00 MiB  
  Allocatable           yes  
  PE Size               4\.00 MiB  
  Total PE              2746175  
  Free PE               2221887  
  Allocated PE          524288  
  PV UUID               5591nx\-wUHR\-QwAq\-Ywc3\-BSkq\-PCT8\-07tmsf

  


On the master node, run the wipefs command to clear the volume of all data and shut down teh node.

root@aus1\-qz2\-rm1\-rk018\-s35:\~\# **sudo wipefs \-\-all \-\-force /dev/sda**  
/dev/sda: 8 bytes were erased at offset 0x00000218 (LVM2\_member): 4c 56 4d 32 20 30 30 31  
/dev/sda: 8 bytes were erased at offset 0xa79cffffe00 (gpt): 45 46 49 20 50 41 52 54  
root@aus1\-qz2\-rm1\-rk018\-s35:\~\# **sudo shutdown \-h now**  
Connection to 192\.168\.18\.155 closed by remote host.  
Connection to 192\.168\.18\.155 closed.

#### On the deploy server:

The mzone.yml file must be modified to remove the varlibdocker entry (the lines to be removed are in red )

Remove varlibdocker entry from mzone file:

**lvm\_groups**:  
  \- **vgname**: disk1  
    **disks**: /dev/sda  
    **create**:true  
    **lvnames**:  
      \- **lvname**: varlibetcd  
        **size**: 1t  
        **create**:true  
        **filesystem**: ext4  
        **mount**:true  
        **mount\_point**: /var/lib/etcd  
        **mount\_options**:'defaults,noatime'  
        **related\_services**:'etcd'

      \- **lvname**: varlibdocker  
        **size**: 1t  
        **create**:true  
        **filesystem**: ext4  
        **mount**:true  
        **mount\_point**: /var/lib/docker  
        **mount\_options**:'defaults,noatime'  
        **related\_services**:'docker'

**On the deploy server:**

**NOTE:  You will need to have defined the version variables with the bundles you want to deploy.**

**nsr@aus1\-qz2\-rm1\-rk018\-s31**:**\~**$ **${XCAT\_DOCKER//MZNAME/$MZONE}/hostos\-boot\-release:${HOSTOS\_BOOT\_VERSION} nextgen\-hostos\-boot \-\-force\-reboot \-n aus1\-qz2\-rm1\-rk018\-s35**  
Do you want to continue with the nextgen\-hostos\-boot operation to mzone70z on 1 nodes? \[y/N] y  
Running nextgen\-hostos\-boot on release hostos\-boot\-release\_1\.1\.2\-20200619T194053Z\_650d092 using payload hostos\-boot\_bionic\-4\.15\.0\.1046\-20200125\-16190ad and hostos\-boot\-tools\_0\.1\.0\-e75dd1e .

**nsr@aus1\-qz2\-rm1\-rk018\-s31**:**\~**$ **${HOSTOS\_DOCKER}/hostos\-config\-release:${HOSTOS\_CONFIG\_VERSION} nextgen\-hostos\-config \-n aus1\-qz2\-rm1\-rk018\-s35**  
Do you want to continue with the nextgen\-hostos\-config operation to mzone70z on 1 nodes? \[y/N] y  
Running nextgen\-hostos\-config on release hostos\-config\-release\_1\.7\.5\-20200717T175915Z\_39564c1 using payload hostos\-config\_0\.1\.0\-d6f3586 and hostos\-config\-tools\_0\.1\.0\-118c223 ...

**nsr@aus1\-qz2\-rm1\-rk018\-s31**:**\~**$ **${HOSTOS\_DOCKER}/hostos\-base\-os\-sw\-release:${HOSTOS\_BASE\_OS\_SW\_VERSION} nextgen\-hostos\-upgrade \-n aus1\-qz2\-rm1\-rk018\-s35**  
Do you want to continue with the nextgen\-hostos\-upgrade operation to mzone70z on 1 nodes? \[y/N] y  
Running nextgen\-hostos\-upgrade on release hostos\-base\-os\-sw\-release\_1\.1\.11\-20200703T194732Z\_731d7ab using payload base\-os\-sw\_0\.1\.0\-b79871c and hostos\-upgrade\-tools\_0\.1\.0\-f44d5dc ...

**nsr@aus1\-qz2\-rm1\-rk018\-s31**:**\~**$ **${HOSTOS\_DOCKER//MZNAME/$MZONE}/hostos\-base\-net\-sw\-release:${HOSTOS\_BASE\_NET\_SW\_VERSION} nextgen\-hostos\-upgrade \-n aus1\-qz2\-rm1\-rk018\-s35**  
Do you want to continue with the nextgen\-hostos\-upgrade operation to mzone70z on 1 nodes? \[y/N] y  
Running nextgen\-hostos\-upgrade on release hostos\-base\-net\-sw\-release\_1\.5\.0\-20200720T183554Z\_ad2c813 using payload base\-net\-sw\_0\.1\.0\-7727fd5 and hostos\-upgrade\-tools\_0\.1\.0\-f44d5dc ...

**nsr@aus1\-qz2\-rm1\-rk018\-s31**:**\~**$ **${HOSTOS\_DOCKER//MZNAME/$MZONE}/hostos\-nextgen\-os\-sw\-release:${HOSTOS\_NEXTGEN\_OS\_SW\_VERSION} nextgen\-hostos\-upgrade \-n aus1\-qz2\-rm1\-rk018\-s35**  
Do you want to continue with the nextgen\-hostos\-upgrade operation to mzone70z on 1 nodes? \[y/N] y  
Running nextgen\-hostos\-upgrade on release hostos\-nextgen\-os\-sw\-release\_1\.1\.12\-20200624T123632Z\_11c9cf8 using payload nextgen\-os\-sw\_0\.1\.0\-d798371 and hostos\-upgrade\-tools\_0\.1\.0\-f44d5dc ...

**nsr@aus1\-qz2\-rm1\-rk018\-s31**:**\~**$ **${HOSTOS\_DOCKER}/hostos\-config\-release:${HOSTOS\_CONFIG\_VERSION} nextgen\-hostos\-config \-n aus1\-qz2\-rm1\-rk018\-s35**  
Do you want to continue with the nextgen\-hostos\-config operation to mzone70z on 1 nodes? \[y/N] y  
Running nextgen\-hostos\-config on release hostos\-config\-release\_1\.7\.5\-20200717T175915Z\_39564c1 using payload hostos\-config\_0\.1\.0\-d6f3586 and hostos\-config\-tools\_0\.1\.0\-118c223 ...

#### On master node:

Confirm the changes.

root@aus1\-qz2\-rm1\-rk018\-s35:\~\# **df \-h**  
Filesystem                    Size  Used Avail Use% Mounted on  
/dev                          249G     0  249G   0% /dev  
rootfs                         20G  3\.1G   17G  16% /  
tmpfs                         250G     0  250G   0% /dev/shm  
tmpfs                         250G  4\.8M  250G   1% /run  
tmpfs                         5\.0M     0  5\.0M   0% /run/lock  
tmpfs                         250G     0  250G   0% /sys/fs/cgroup  
tmpfs                         250G  120K  250G   1% /tmp  
tmpfs                         250G     0  250G   0% /var/tmp  
/dev/mapper/disk1\-varlibetcd 1007G   77M  956G   1% /var/lib/etcd  
tmpfs                          50G     0   50G   0% /run/user/0  
root

root@aus1\-qz2\-rm1\-rk018\-s35:\~\# lvdisplay  
  \-\-\- Logical volume \-\-\-  
  LV Path                /dev/disk1/varlibetcd  
  LV Name                varlibetcd  
  VG Name                disk1  
  LV UUID                eeHEhe\-qXNj\-yjae\-jhcH\-SWQd\-xHVs\-9Ccdh0  
  LV Write Access        read/write  
  LV Creation host, time aus1\-qz2\-rm1\-rk018\-s35, 2020\-07\-23 14:59:36 \+0000  
  LV Status              available  
  \# open                 1  
  LV Size                1\.00 TiB  
  Current LE             262144  
  Segments               1  
  Allocation             inherit  
  Read ahead sectors     auto  
  \- currently set to     256  
  Block device           253:0  
    
root@aus1\-qz2\-rm1\-rk018\-s35:\~\# pvdisplay  
  \-\-\- Physical volume \-\-\-  
  PV Name               /dev/sda  
  VG Name               disk1  
  PV Size               \<10\.48 TiB / not usable 4\.00 MiB  
  Allocatable           yes  
  PE Size               4\.00 MiB  
  Total PE              2746175  
  Free PE               2484031  
  Allocated PE          262144  
  PV UUID               Kjmnpy\-B8zg\-dymW\-t4gQ\-sLQq\-Ie0l\-RmzDSD  
 

   
 





## Comments:





| Hi [Nolan Ring](https://confluence.swg.usma.ibm.com:8445/display/~nolan.ring@ibm.com).  This looks like a good set of instructions for Gen1 HW, but as [Malcolm Allen\-Ware](https://confluence.swg.usma.ibm.com:8445/display/~mware@us.ibm.com) pointed out, this wouldn't work for Gen2 HW with the MD array.  Also, I assume this is only for the k8s etcd masters, since they are the only ones with persistent storage intalled, but one request relative to that, could you put a few context notes in an overview section at the top of the doc, describing things like that?  What is is intended to be used for, what scenarios it needs executed in, etc.?  thanks...    Posted by Kevin.Hughes@ibm.com at Jul 24, 2020 17:30 |
| --- |
| Great feedback [Kevin Hughes](https://confluence.swg.usma.ibm.com:8445/display/~Kevin.Hughes@ibm.com).  Apologies for the delay in getting back to you on this.  I will certainly add some context notes, and would be happy to add info for the Gen2 HW.  Are you the person to ask about Gen2 HW info?    Posted by nolan.ring@ibm.com at Aug 04, 2020 13:54 |
| Sure can be a start point, I've been working with it a bit now (smile)     Posted by Kevin.Hughes@ibm.com at Aug 04, 2020 14:01 |



 


Document generated by Confluence on Jul 15, 2024 13:04


[Atlassian](https://www.atlassian.com/)


 


