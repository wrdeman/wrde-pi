# Kubernetes on Raspberry PI

I have followed this blog: https://uthark.github.io/post/2020-09-02-installing-kubernetes-raspberrypi/ and where possible converted into ansible.

## Prepare the PIs

First install Ubuntu image. This is a bit of a chore and I followed: https://ubuntu.com/download/raspberry-pi

Then fixed the IP address on the PIs. I could have probably done this using ansible but ...

In `/etc/netplan/50-cloud-init.yaml`

```
      dhcp4: no
      addresses:
        - 192.168.121.221/24
      gateway4: 192.168.121.1
      nameservers:
          addresses: [8.8.8.8, 1.1.1.1]
```
and in `/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg` add
```
network: {config: disabled}
```

and finally apply these changes:
```
sudo netplan apply
```

In `/etc/hosts` give the PIs nice names:

```bash
192.168.0.30 k8-worker3
192.168.0.31 k8-master
192.168.0.39 k8-worker2
192.168.0.40 k8-worker1
192.168.0.29 k8-nfs
```

## Ansible

Next we need to install a docker and kubernetes. 

### cgroups

To get docker to work on the PIs we need to enable cgroups limits and this is achieved 
 

## kubernetes

### CNI
curl https://docs.projectcalico.org/manifests/calico.yaml -O | kubectl apply -f calico.yaml

### Helm
helm repo add traefik https://helm.traefik.io/traefik

helm repo update

helm install traefik traefik/traefik


https://nick.sarbicki.com/blog/deploy-django-celery-on-k8s/

https://medium.com/@wkrzywiec/how-to-deploy-application-on-kubernetes-with-helm-39f545ad33b8

### NFS

Create an ephemeral NFS storage on a raspberry pi.

1. Install nfs client on a raspberry pi. https://opensource.com/article/20/5/nfs-raspberry-pi

2. Set up nfs client provisioner: https://opensource.com/article/20/6/kubernetes-nfs-client-provisioning
    a) git clone https://github.com/kubernetes-incubator/external-storage.git
    b) cd nfs-client/deploy
    c) kubectl create -f rbac.yaml
    d) kubectl create -f deployment-arm.yaml <- edit for IP and path 


https://www.raspberrypi.org/documentation/configuration/nfs.md




## Requirements
* ansible-galaxy collection install ansible.posix
* ansible
