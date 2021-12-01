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

**On client Machine**
