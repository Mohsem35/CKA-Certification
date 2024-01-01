### Kubernetes Setup

kubeadm tool helps us set up a multi-node cluster using kubernetes best practices.

![kubeadm-stacked-color(2)](https://github.com/Mohsem35/CKA-Certification/assets/58659448/3ef6c714-079f-49ab-93b5-65652e341e69)


#### Steps
1. Need to have **multiple systems or VMs provisioned**
    - we are gonna need a couple of nodes for our kubernetes cluster. this can be physical/virtual machines.
    - designate one node as a master and the rest as worker nodes

Resources:
[DevOps Box: Vagrant with KVM](https://joachim8675309.medium.com/devops-box-vagrant-with-kvm-d7344e79322c)

2. **Install container runtime** on the hosts
    - we will specifically be using containerD on all of the nodes.

Resources: 
- [Container Runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)

- [Getting started with containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)

3. Install kubeadm tool on all the nodes
    - kubeadm tool helps us bootstrap the kubernetes solution by installing all of the necessary components on the right nodes in the right order 
4. Initialize master server
    - all of the required components are installed and configured on the master
5. POD network
    - kubernetes requires a special networking solution between master and worker nodes
6. Join master node(for worker nodes)


[Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
