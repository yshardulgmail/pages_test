---
layout: default
title: RIAS failures using jump pod
parent: Debug
grand_parent: Developer Information
---

To view or manipulate RIAS-G CRD resources via regional-extension-server, you need to spin up a jump pod in the RIAS namespace on an IKS cluster. In most environments, this already exists. However, in case it does not, this guide details how to create a new one.

## Using a script to create a jump pod
For information about using the `create-jumpod.sh` script to create a jump pod, see <https://github.ibm.com/cloudlab/pso_vpc/blob/master/jumpod-creation/README.md>.

## Manually creating a jump pod

#### Target the appropriate IKS cluster:
```
$ export KUBECONFIG=<IKS_CLUSTER_KUBECONFIG_PATH>
Alternatively:
$ $(ibmcloud cs cluster-config <IKS Cluster name> --export)
$ export IKS_CLUSTER=<IKS_CLUSTER_KUBECONFIG_PATH> (ie. "rias-ng-us-south-dal-dev99-etcd")
$ ibmcloud cs cluster config -c $IKS_CLUSTER [-q]
```

#### View regional-extension-server's kubeconfig secret


You will need this value in a moment. This is a **kubernetes secret value** - do not post the contents anywhere.

```bash
kubectl -n rias get secret regional-extension-server-client-kubeconfig -o yaml | grep 'kubeconfig:' | awk '{print $2}' | base64 -d
```

#### Spin up the jump pod in the rias namespace

```bash
kubectl -n rias run -it jumpod-new --image=ubuntu:latest -- bash

# Exit the container:
exit

# Now go back in:
kubectl -n rias exec -it <jumpod-name> -- /bin/bash
```

### From inside the jump pod

You should still be in the jumpod from above, after having created it, exited, and then re-entered a second time. The second re-entry is crucial, as otherwise this will need to be redone.

#### Install vim and curl

```bash
apt-get update && apt-get install -y vim curl
```

#### Make the kube and bin directories

```bash
mkdir ~/.kube ~/bin
```

#### Generate the KUBECONFIG for inside the jump pod

**_Make sure to replace the `<NUMERIC_TOKEN_FROM_SECRET>` with your `token` value from the step where we [viewed the kubeconfig for later]([#### View regional-extension-server's kubeconfig secret](https://pages.github.ibm.com/cloudlab/hostos-docs/docs/developer_information/debug/troubleshooting-rias-failures-using-jump-pod.html#view-regional-extension-servers-kubeconfig-secret))_**.

```bash
cat << EOF > ~/.kube/config
---
apiVersion: v1
kind: Config
clusters:
- name: regional-extension-server
  cluster:
    insecure-skip-tls-verify: true
    server: https://regional-extension-server.rias.svc:6443
users:
- name: rias
  user:
    token: <NUMERIC_TOKEN_FROM_SECRET>
contexts:
- name: rias
  context:
    cluster: regional-extension-server
    user: rias
current-context: "rias"
EOF
```

#### Download kubectl

```bash
curl -o ~/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod a+x ~/bin/kubectl
```

#### If Desired, install jq

```bash
apt-get install -y jq
```

#### Configure the jump pod to use the kubeconfig and installed packages

```bash
echo 'export KUBECONFIG=~/.kube/config' >> ~/.bashrc
echo 'export PATH=~/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### Test you can see resources now

```bash
kubectl get crds
kubectl get vpcs --all-namespaces
kubectl get securitygroups --all-namespaces
# etc..
```

### Exit the jumpod

```bash
exit
```

### Delete jump pod
1. Confirm you can still see the jump pod:  
   `kubectl get pods -n rias |grep jumpod-new # or replace with jump pod name`
2. If required, log in again to ibmcloud.
3. Delete the jump pod:  
`kubectl delete pod <PODNAME> -n rias`

## Troubleshooting RIAS smoke failed Tests
In situation where the VPCs don't get deleted, this script will help you to get its dependent objects such as subnets, instance specs , floatingips, public gateways, endpoint gateways, reservedips and loadbalancers. 
Generally you want to clean up from the bottom, but there are some exceptions.

```
#!/bin/bash

echo VPCs:
kubectl get vpcs -A -l 'ServiceVCN!=true'
echo

echo Subnets:
kubectl get subnets -A -l 'ServiceVCN!=true'
echo

echo "Instances (instancespecs):"
kubectl get instancespecs -A
echo

echo "Floating IPs (floatingips):"
kubectl get floatingips -A -lservice=false
echo

echo "Public Gateways (publicgateways):"
kubectl get publicgateways -A
echo

echo "Endpoint Gateways (endpointgateways):"
kubectl get endpointgateways -A
echo

echo "Reserved IPs (reservedips):"
kubectl get reservedips -A -l 'Owner!=provider'
echo

echo "DNLBs (loadbalancers):"
kubectl get loadbalancers -A
echo

exit 0
```
## commands 

```
To delete VPCs
kubectl -n $NS delete vpc $VPC


To delete instancespecs
kubectl -n $NS delete instancespecs $instancespecs?

To get VPC details -
kubectl -n 6ad0af1f7d5d7ea18db5c3c17d129934 get vpc r134-60c84949-6535-4b53-8235-50262e4315ab -o yaml

To get router (projection finaliser) (to be run from master node of mzone)
kubectl -n 6ad0af1f7d5d7ea18db5c3c17d129934 get router r134-60c84949-6535-4b53-8235-50262e4315ab -o yaml
```


