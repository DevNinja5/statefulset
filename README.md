# StatefulSet - Kubernetes

## Implementing StatefulSet with static volume provisioning with NFS server on a local cluster with 2 minikube nodes

### PreRequestisites  - Ubuntu_20.04

> Note: In this doc I'm using 2 minikube nodes provisioned on a Ubuntu 20.04


**On Host Machine**

1. NFS host server utils

```bash
sudo apt update
sudo apt install nfs-kernel-server
```

2. Make a directory to persist volume on host machine

> for 3 replicas of statefulset
```bash
sudo mkdir -p /tmp/nfs/kubedata/{pv0,pv1,pv2}
sudo chmod -R 777 /tmp/nfs/kubedata
```

3. Check directory structure of volume 

```bash
cd /tmp/nfs
tree
```

It should look like this

```text
.
└── kubedata
    ├── pv0
    ├── pv1
    └── pv2

4 directories, 0 files
```

4. Grant NFS Share Access to Client Systems

```bash
sudo vi /etc/exports
```

Add your Directory and hosts with policies. (* for all clients)

```
/tmp/nfs/kubedata        *(rw,sync,no_subtree_check,insecure)

```
It should look like this

```
# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#

/tmp/nfs/kubedata        *(rw,sync,no_subtree_check,insecure)

```

5. Export NFS share dir and restart NFS service

```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

6. Allow NFS Access through the Firewall
> My minikube node network gateway is 192.168.49.0/24

> check your gateway $minikube ip  : It should look like 192.168.49.2 then gateway is 192.168.49.0/24
```bash
sudo ufw allow from 192.168.49.0/24 to any port nfs
sudo ufw enable
sudo ufw status
```
**On client Machine**
