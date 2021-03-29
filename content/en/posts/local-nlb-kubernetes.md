+++
title = "Local NLB for Kubernetes"
description = ""
tags = [
    "kubernetes",
    "metallb",
    "loadbalancer",
]
date = "2020-06-02"
categories = [
    "Kubernetes",
]
menu = "main"
+++

I decided to build a Kubernetes infra from scratch using some virtual machines inside my laptop, because I think it's the better way to study for CKA and to know better how Kubernetes works (and since I also don't want to pay a few dollars to a cloud provider, because exchange rates have skyrocketed in Brazil :-) )

The baddest thing to use this kind of infra instead cloud providers is try to expose a service using the LoadBalancer type. Inside the cloud provider such thing is quite easy.

But since I discovered the [MetalLB](https://metallb.universe.tf/) project I don't have such problem anymore.

MetalLB provides a network load balancer implementation for bare metal kubernetes clusters, using standard routing protocols (since Kubernetes doesn't offer one), in other words, I'm able to load balancing requests using the kubernetes way (the right way ;-) ), not using an external nginx to loadbalancing nodeport services.

Below the steps which I did in my environment.

**Environment**
- 4 libvirt VM's w/ CentOS 7 (provisioned with Vagrant)
- 1 master node
- 3 workers nodes
- Kubernetes 1.18
- MetalLB 0.9.3

**Preparation**
First I needed to check, as documentation of MetalLB asks for, if I need to enable the strict ARP mode for kube-proxy. Since Kubernetes 1.14.2 for kube-proxy in ipvs mode, we need to enable strict ARP.

To check it:
```
kubectl describe cm -n kube-system kube-proxy | grep -i strictarp
```

If you get this return:
```yaml
  strictARP: false
```

You need to enable strictARP, to do that use those commands:
```
kubectl get cm kube-proxy -n kube-system -o json > strict.json && sed 's/strictARP: false/strictARP: true/g' strict.json && kubectl replace -f strict.json; rm -f strict.json
```

**Installation**
Now we are able to install, the documentation provide to us two ways to install, the manifest way and the kustomize way, I chose apply the manifests:

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.3/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.3/manifests/metallb.yaml
```

We need to generate a secret also (used to encrypt the communication between the speakers)
```
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```
After that according the docs those components will be deployed:
> This will deploy MetalLB to your cluster, under the metallb-system namespace. The components in the manifest are:

> The metallb-system/controller deployment. This is the cluster-wide controller that handles IP address assignments.
> The metallb-system/speaker daemonset. This is the component that speaks the protocol(s) of your choice to make the services reachable.
> Service accounts for the controller and speaker, along with the RBAC permissions that the components need to function.

At last we need to configure MetalLB defining and deploying a configmap, I used the simplest mode (according the docs, but in my case was true), the Layer 2 mode:

`metallb-config.yaml`
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.122.2-192.168.122.10
```
```
kubectl apply -f metallb-config.yaml
```

After that MetalLB was able to allocate those IP's (they are not random, they are one part of my available IP's from the virtual network builded by libvirt) as a load balancer.

And now we can test.