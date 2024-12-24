---
layout: default
title: VPC service on Dev Mzones 
parent: Developer Information
---
## Introduction

With IBM Cloud® Virtual Private Cloud (VPC), you can use the UI, CLI, and API to quickly provision virtual server instances for VPC with high network performance. VPC infrastructure contains a number of Infrastructure\-as\-a\-Service (IaaS) offerings, including Virtual Servers for VPC.

## Pre\-requisites

We need the following pre\-requsites completed to get started with utilising the VPC services on any HostOS Dev mzone

1. Test account registered at IBM Cloud staging instance <https://test.cloud.ibm.com/>  
Visit <https://test.cloud.ibm.com/registration> and register test account using your W3 id
2. Generate api key for cli based authentication to IBM Cloud staging account  
Create a new api key at <https://test.cloud.ibm.com/iam/apikeys> by clicking on the Create button ![](attachments/383956559/383956570.png)  
This api key will be used for authenticating to IBM Cloud staging using the ibmcloud\-cli tool
3. Install ibmcloud\-cli tool on your test machine/desktop  
Follow the steps as [https://cloud.ibm.com/docs/cli?topic\=cli\-install\-ibmcloud\-cli](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli) to install and setup ibmcloud ci tool on your test machine
4. Install infrastructure plugin for ibmcloud cli 



```
ibmcloud plugin install vpc-infrastructure
```

## Getting Started

Please follow the listed steps to get started with creating and managing VPC resources on a HostOS Dev mzone

## HostOS RIAS Api endpoints

Every HostOS Mzone has a unique RIAS endpoint that needs to be used for creating VPC resources on the corresponding mzone

Following are the list of available RIAS endpoints for HostOS Dev mzones



| Mzone | RIAS Endpoint |
| --- | --- |
| mzone7204 | [https://us\-south\-genesis\-dal\-dev56\-etcd.iaasdev.cloud.ibm.com](https://us-south-genesis-dal-dev56-etcd.iaasdev.cloud.ibm.com) |
| mzone7203 | [https://us\-south\-genesis\-dal\-dev55\-etcd.iaasdev.cloud.ibm.com](https://us-south-genesis-dal-dev55-etcd.iaasdev.cloud.ibm.com) |
| mzone7212 | [https://us\-south\-ng\-mzone7212\.iaasdev.cloud.ibm.com](https://us-south-ng-mzone7212.iaasdev.cloud.ibm.com) |
| mzone230j | [https://us\-south\-genesis\-dal\-dev29\-etcd.iaasdev.cloud.ibm.com](https://us-south-genesis-dal-dev29-etcd.iaasdev.cloud.ibm.com) |
| mzone2114 | [https://us\-south\-genesis\-dal\-dev28\-etcd.iaasdev.cloud.ibm.com](https://us-south-genesis-dal-dev28-etcd.iaasdev.cloud.ibm.com) |

## IBM Cloud cli login

Please follow the underlying steps to login to IBM Cloud staging using the ibmcloud cli

1. Set the following environment variables

      ```bash
      export zone=us-south; export IBMCLOUD_IS_NG_API_ENDPOINT=<RIAS_API_ENDPOINT_FOR_MZONE>
      ```

2. Login with ibmcloud cli

      ```bash
      ibmcloud login --sso -r us-south -a test.cloud.ibm.com --apikey <YOUR_API_KEY>
      ```

## Creating VPC,Subnets and more

Before we create any resources such as VSI's , we need to setup a VPC, Subnet, Security Groups and ssh keys for connectivity/authentication

The following script helps to automate creation of the VPC and other network entities  
  
```bash
#!/bin/bash

# Define VPC name.Should be unique
name=YOUR_VPC_NAME
# Get available zones by using ibmcloud is zones, ex us-south-3
zone=YOUR_ZONE
# Create ssh key pair and substitute the public key here
key=YOUR_PUBLIC_KEY

ibmcloud is vpcc $name-vpc1
VPC_ID="$(ibmcloud is vpcs | grep $name-vpc1 | awk -F" " '{print $1}')"
rm ./VPC.txt
echo $VPC_ID > ./VPC.txt

RG_ID="$(ibmcloud resource groups | grep ACTIVE | awk '{print $2}')"
rm ./RG.txt
echo $RG_ID > ./RG.txt

ibmcloud is vpc-address-prefix-create $name-prefix-1 $VPC_ID  $zone 192.168.128.0/18

ibmcloud is subnet-create $name-sub1 $VPC_ID $zone --ipv4-cidr-block 192.168.128.0/18

sleep 5

ibmcloud is subnets | grep $name-sub1 | awk -F" " '{print $1}' > SUBNET_$zone.txt


SUB1="$(awk {'print $1'} SUBNET_$zone.txt)"

sleep 5

ibmcloud is security-group-create $name-sg $VPC_ID
SG_ID="$(ibmcloud is security-groups | grep $name-sg | awk -F " " '{print $1}')"
rm ./SG.txt
echo $SG_ID > ./SG.txt

sleep 5

ibmcloud is security-group-rule-add $SG_ID inbound   tcp --port-min 22 --port-max 22
ibmcloud is security-group-rule-add $SG_ID outbound  tcp --port-min 22 --port-max 22

ibmcloud is security-group-rule-add $SG_ID inbound  icmp
ibmcloud is security-group-rule-add $SG_ID outbound icmp

ibmcloud is security-group-rule-add $SG_ID inbound  udp
ibmcloud is security-group-rule-add $SG_ID outbound udp

ibmcloud is security-group-rule-add $SG_ID outbound all
ibmcloud is security-group-rule-add $SG_ID inbound all


sleep 5
KEY_ID="$(ibmcloud is keyc hp-key  "$key" | grep ID |  awk -F" " '{print $2}')"
rm ./KEY.txt
echo $KEY_ID > ./KEY.txt
```

  
Please follow the underlying steps to create and setup VPC components:

1. Create a rsa key pair. This key will be registered with VPC and will be used for ssh connectivity to underlying VSI's

    ```bash
    ssh-keygen -t rsa
    ```

2. Update the vpc name,zone and key details in the bash script shared above and execute script


    ```bash
    # Define VPC name. Name should be unique
    name=YOUR_VPC_NAME
    # Get available zones by using command `ibmcloud is zones`, ex us-south3
    zone=YOUR_ZONE
    # Create ssh key pair and substitute the public key here
    key=YOUR_PUBLIC_KEY
    ```

# Creating VSI

Please use the following automation script to create VSI as part of your VPC


```bash
# Get available zones by using ibmcloud is zones
ZONE_NAME=us-south-3

# Use a profile class with minimum hardware footprint
PROFILE_NAME=cx2-2x4

# Check available images using ibmcloud is images
IMAGEID=r134-1025e040-7d6f-408c-b4db-6156dc986fc7 ;

# Unique instance name
INSTANCE_NAME=YOUR_INSTANCE_NAME


for inst in {0..0};
do

    VPC_ID="$(sort -R VPC.txt | head -n1)"
    SUBNET_ID="$(sort -R SUBNET_$ZONE_NAME.txt | head -n1)"

    SG_ID="$(sort -R SG.txt | head -n1)"

    KEY_ID="$(sort -R KEY.txt | head -n1)"
    RG_ID="$(sort -R RG.txt | head -n1)"

    ibmcloud is inc $INSTANCE_NAME-$RANDOM $VPC_ID $ZONE_NAME $PROFILE_NAME $SUBNET_ID --sgs $SG_ID --image $IMAGEID  --keys $KEY_ID --resource-group-id $RG_ID
done
```

  


Please follow the underlying steps to create VSI and assign it a Floating IP address for SSH access:

1. Identify the VSI Guest OS image to use by chekcing the ibmcloud image catalog as follows:

    ```bash
    ibmcloud is images
    ```

Select an image thats shown as available. We need to grab the id of the image such as **r134\-d07c19ac\-6cf3\-45b7\-a335\-01b7a6b06427**for it to be associated with a VSI
2. Update the vsi name,zone, profile class and image id details in the bash script shared above and execute script  

``bash
      # Get available zones by using `ibmcloud is zones`
      ZONE_NAME=us-south-3
      # Use a profile class with minimum hardware footprint
      PROFILE_NAME=cx2-2x4
      # Check available images using `ibmcloud is images`
      IMAGEID=r134-1025e040-7d6f-408c-b4db-6156dc986fc7 ;
      # Unique instance name for the VSI
      INSTANCE_NAME=YOUR_INSTANCE_NAME
      ```
3. After successful VSI creation you will be shown the vsi instance id such as **230j\_efcfbbad\-be83\-488e\-b20c\-3e7a67ed2eee**The vsi id is always prefixed with the mzone name. The vsi details may be viewed using the following command

  ```bash
      ibmcloud is instance <VSI_INSTANCE_ID>eg. ibmcloud is instance 230j_efcfbbad-be83-488e-b20c-3e7a67ed2eee
    ```

4. ## Assign a Floating IP address to the vsi for ssh access using the following script



```bash
instNAME=Default

ibmcloud is ins | grep $instNAME  > INSTANCES

for instID in `cat INSTANCES | awk '{print $1}'`;
do
  echo $instID
  NIC_ID="$(ibmcloud is instance-network-interfaces $instID | grep primary | awk '{print $1}')"
  ibmcloud is floating-ip-reserve lm-$RANDOM  --nic $NIC_ID
done


```

Please ignore any error if you notice as this script tries to add a floating ip for all VSI's under your VPC.So VSI's that already have a floating ip will return error


## Attaching Data Volumes to a VSI

Please follow the underlying steps to create and attach a data volume to an existing VSI

1. Create the data volume

```bash
ibmcloud is volume-create <VOLUME_NAME> general-purpose <ZONE> --capacity 100
```

After successful creation the data volume id will be returned which needs to be used for attaching it to a existing VSI
2. Attach data volume to a vsi

  ```bash
    ibmcloud is instance-volume-attachment-add <ATTACHMENT_NAME> <VSI_INSTANCE_ID> <DATA_VOLUME_ID> --auto-delete true
   ```

3. You can check if the volume was attached by querying the vsi instance details:

      ```bash
      ibmcloud is instance <VSI_INSTANCE_ID>
      ```

## Additional Useful commands

1. Stop a VSI

      ```bash
      ibmcloud is instance-stop <VSI_INSTANCE_ID>
      ```

2. Start a VSI

      ```bash
      ibmcloud is instance-start <VSI_INSTANCE_ID>
      ```

3. IBM Cloud cli vpc commands help

```bash
ibmcloud is help
ibmcloud is <command> help
```

## VPC Cleanup commands

  


Please follow the underlying steps to cleanup all vpc resources on a given mzone

* Delete all VSI's



```
ibmcloud is ind $(ibmcloud is instances | grep "^<MZONE_NAME>" | awk '{print $1}' | xargs) -f

eg. ibmcloud is ind $(ibmcloud is instances | grep "^230j" | awk '{print $1}' | xargs) -f
```
* Delete all subnets



```
ibmcloud is subnet-delete $(ibmcloud is subnets | grep "^<MZONE_NAME>" | awk '{print $1}' | xargs) -f

eg. ibmcloud is ind $(ibmcloud is instances | grep "^230j" | awk '{print $1}' | xargs) -f
```
* Delete all VPC



```
ibmcloud is vpc-delete $(ibmcloud is vpcs | grep "^r134" | awk '{print $1}' | xargs) -f
```
* Delete key



```
ibmcloud is key-delete $(ibmcloud is keys | grep "^r134" | awk '{print $1}' | xargs) -f
```






 


