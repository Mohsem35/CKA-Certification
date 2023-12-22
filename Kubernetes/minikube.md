
[How to Install KVM on Ubuntu 20.04](https://phoenixnap.com/kb/ubuntu-install-kvm)


[Steps to Install Minikube On Ubuntu and Deploy Applications](https://medium.com/@dileepjallipalli/how-to-install-and-use-minikube-for-k8s-ee30a75ce8bc)

```shell
apt-get install curl wget apt-transport-https -y
apt-get install virtualbox virtualbox-ext-pack
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

```shell
minikube start
😄  minikube v1.32.0 on Ubuntu 20.04 (kvm/amd64)
✨  Automatically selected the virtualbox driver. Other choices: none, ssh
💿  Downloading VM boot image ...
    > minikube-v1.32.1-amd64.iso....:  65 B / 65 B [---------] 100.00% ? p/s 0s
    > minikube-v1.32.1-amd64.iso:  292.96 MiB / 292.96 MiB  100.00% 2.25 MiB p/
👍  Starting control plane node minikube in cluster minikube
💾  Downloading Kubernetes v1.28.3 preload ...
    > preloaded-images-k8s-v18-v1...:  403.35 MiB / 403.35 MiB  100.00% 180.83 
🔥  Creating virtualbox VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
🤦  StartHost failed, but will try again: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory
🔥  Creating virtualbox VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
😿  Failed to start virtualbox VM. Running "minikube delete" may fix it: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory

❌  Exiting due to HOST_VIRT_UNAVAILABLE: Failed to start host: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory
💡  Suggestion: Virtualization support is disabled on your computer. If you are running minikube within a VM, try '--driver=docker'. Otherwise, consult your systems BIOS manual for how to enable virtualization.
🍿  Related issues:
    ▪ https://github.com/kubernetes/minikube/issues/3900
    ▪ https://github.com/kubernetes/minikube/issues/4730
```