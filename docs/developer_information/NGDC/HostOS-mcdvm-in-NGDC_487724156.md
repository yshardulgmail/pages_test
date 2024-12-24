---
layout: default
title:  mcdvm in NGDC 
parent: NGDC
grand_parent: Developer Information
---
## **Minimal Common Denominator VM (mcdvm)**

[Qcow images on artifactory](https://na-public.artifactory.swg-devops.com/ui/native/wcp-genctl-sandbox-generic-local/cloudlab/hostos/canonical-images/) (may need to load this page twice!)

Cloud\-init user\-data.txt file

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-


```bash
#cloud-config
debug:
 verbose: true
hostname: ngdc-mcdvm
chpasswd:
 list: |
  root:passw0rd!!!
 expire: False
ssh_pwauth: True
disable_root: false

users:
 - name: sysop
  sudo: ['ALL=(ALL) NOPASSWD:ALL']
  groups: sudo
  shell: /bin/bash
  ssh-authorized-keys:
   - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDavc2S/6Mz0iefICsoEZAfJTdIEZ3Ga1vO5djq8AHWidTZXB6LFyd1lKnvbjjxsF1Av1KGOe+iL58JTo1ND6zEJdNXK4IquJO5PNnMrkRKOpupKu95WHYOQWIRX8MZ/SvH/snkrexYT02qjMeLGMfvCepVP8p899TWi2sLl/HrGuUleWObvFy/u1+mYB+G6mGm4a1nmkbxWdanzq9qzSgeLf/di4hdr5P+meOdHAyexS/1fzrqvphJAeRqVCcK/WbYxGeUvMvKZfM0qgiTA8FSH9vQw2YA2CXUvBy6RD2A9VNrDlliLNt5W5ZTTw1UMgyNyaAO2362Zedr09kOs7GN

runcmd:
 - mkdir -p /etc/genesis
write_files:
 - path: /etc/ssh/sshd_config
  content: |
   PubkeyAcceptedKeyTypes=+ssh-rsa
  append: true
 - path: /etc/genesis/release_bundles
  content: |
   hostos-boot-release:6.2.0-20230823T060058Z_35a1eec

runcmd:
    - service sshd restart

final_message: "System up for $UPTIME seconds"
```
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-

## **Testing with Fyre VM**

On a Fyre VM, create a seed.img using user\-data.txt

* % cloud\-localds seed.img user\-data.test

Boot up qcow image with seed.img to setup sysop login

* % qemu\-system\-x86\_64 \-machine accel\=kvm \-drive media\=disk,if\=ide,index\=0,file\=hostos\-boot\_jammy\-\<version\>.qcow2 \-drive if\=virtio,format\=raw,file\=seed.img \-nographic \-m 4070
* shutdown the vm after verifying that sysop is setup. (Network is strange, it's not talking to the host.)

Bring up the VM again with virt\-install, this allows the host to talk to the VM on the 192\.168\.122\* network

* Increase the qcow2 disk size to make room for allow additional release bundles to be installed
	+ % truncate \-s \+20G hostos\-boot\_jammy\-\<version\>.qcow2
	+ % virt\-install \-\-name mcdvm\-1 \-\-memory 8192 \-\-vcpu 2 \-\-disk ./hostos\-boot\_jammy\-\<version\>.qcow2 \-\-import \-\-os\-variant ubuntu22\.04 \-\-network default \-\-check path\_in\_use\=off
	+ The console may appears to be hung. Open up another terminal, login for the Fyre VM and run the following
		- % virsh list                               \# to see running vm
		- % virsh console  \<domain\>    \# to access the console
* login to the vm through virsth console and setup the network
	+ ip addr add 192\.168\.122\.40/24 dev enps0
	+ ip link set up dev enps0
	+ ping 192\.168\.122\.1  \# this is the virbr0 on the host. it should ping!

Deploy release bundles

* setup the ddt tool deploy environment on your vm, checkout the [onboarding](https://confluence.swg.usma.ibm.com:8445/display/HCP/Onboarding) page (12\. Deployer Tool)
* Define your platform\-inventory/regions/mzone1234\.yml file
* Define your mzone file for ddt
	+ bundles to use: hostos\-config\-release, hostos\-base\-os\-sw\-release, hostos\-post\-config\-release.

## **Accessing NGDC Environment**

Request "fabng\-feddev\-ucnat" from IaaS Access Management in [AccessHub](https://ibm.idaccesshub.com/ECMv6/request/manageRequest).

ssh [tmp\_vmaas\_jump@172\.16\.225\.134](mailto:tmp_vmaas_jump@172.16.225.134); ping Tarek Zegar for password.

From the jump host, ssh into the vm, ping Tarek for a VM

How to deploy with FAST..

How to deploy with docker..


 


