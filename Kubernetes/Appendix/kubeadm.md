
kubelet হচ্ছে একটা agent যে হচ্ছে আমার server এ container চলতেছে কিনা এইসব জিনিষপত্র নিয়া master এর কাছে report করবে 

kubeadm দিয়ে আমরা cluster টা initiate করি 

kubectl হচ্ছে dockercli এর মত একটা tool


### Kubernetes Setup

kubeadm tool is used to bootstrap and manage **production grade** kubernetes cluster

kubeadm tool helps us set up a multi-node cluster using kubernetes best practices.

![kubeadm-stacked-color(2)](https://github.com/Mohsem35/CKA-Certification/assets/58659448/3ef6c714-079f-49ab-93b5-65652e341e69)

<img width="808" alt="Screenshot 2024-01-01 at 7 22 14 AM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/3d0a12cc-f229-4e27-8590-09009c84dc29">

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
    - `Option 2: From apt-get or dnf` -> `Ubuntu` -> [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
     
```shell
sudo apt-get install containerd.io
systemctl status containerd
```

- [cgroup drivers](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#cgroup-drivers)

_Now let's verify what is our init system?_

```shell
ps -p 1

PID TTY          TIME CMD
  1 ?        00:00:45 systemd
```

- To set the cgroup driver to **`systemd`**

[containerd ](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd) -> `Configuring the systemd cgroup driver` -> copy the code block -> vim `/etc/containerd/config.toml` to everynode -> delete all lines from the file -> paste copied lines form the documentation -> `sudo systemctl restart containerd`

> **Note**: In v1.22 and later, if the user does not set the cgroupDriver field under KubeletConfiguration, kubeadm defaults it to `systemd`

3. Install kubeadm tool on all the nodes
    - kubeadm tool helps us bootstrap the kubernetes solution by installing all of the necessary components on the right nodes in the right order 

Resources: 

[Installing kubeadm, kubelet and kubectl](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl)



4. Initialize master server
    - all of the required components are installed and configured on the master

Resources: 

[Creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) -> [Initializing your control-plane node](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node) 

```shell
# run the following command just on the master node
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=172.16.6.18

# control plane successfully initialized
[init] Using Kubernetes version: v1.29.0
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
W0102 08:37:27.611300   79420 checks.go:835] detected that the sandbox image "registry.k8s.io/pause:3.6" of the container runtime is inconsistent with that used by kubeadm. It is recommended that using "registry.k8s.io/pause:3.9" as the CRI sandbox image.
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local swarm-worker2] and IPs [10.96.0.1 172.16.6.18]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [localhost swarm-worker2] and IPs [172.16.6.18 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [localhost swarm-worker2] and IPs [172.16.6.18 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "super-admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[kubelet-check] Initial timeout of 40s passed.
[apiclient] All control plane components are healthy after 42.251448 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node swarm-worker2 as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node swarm-worker2 as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
[bootstrap-token] Using token: jzs39q.nwcq6r957a3q9wsr
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] Configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.16.6.18:6443 --token jzs39q.nwcq6r957a3q9wsr \
	--discovery-token-ca-cert-hash sha256:fca657c6266564a69c2fbdb7152068bf777aa43b95a7c498a3ecbe1e4da5c2cd
```

```shell
# master node
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# connect to the cluster
kubectl get pod
```
5. POD network
    - kubernetes requires a special networking solution between master and worker nodes

Resources:

- [Installing Addons](https://kubernetes.io/docs/concepts/cluster-administration/addons/) 

  - [Networking and Network Policy](https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy) -> [Weave Net](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/) -> [Installation](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation)

```shell
# master node
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml

# get all pods across all namespaces
kubectl get pods -A


NAMESPACE       NAME          READY       STATUS        RESTARTS      AGE
kube-system weave-net-nw9v2    0/2    PodInitializing       0         14s
```

More read:

[Things to watch out for](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-things-to-watch-out-for) -> [Changing Configuration Options](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-changing-configuration-options)

```shell
# master node
kubectl get ds -A

# edit configuration file
kubectl edit ds weave-net -n kube-system

# we have to look for the containers
# spec > template > spec > containers > env
- name: IPALLOC_RANGE
  value: 10.244.0.0/16          # according to the kubeadm init

kubectl get pods -A

# output
NAMESPACE       NAME          READY       STATUS        RESTARTS      AGE
kube-system weave-net-2ztq2    2/2        Runiing           0         16s
```

  
6. Join master node(for worker nodes)

Run the following command to worker nodes

```shell
# worker nodes
kubeadm join 172.16.6.18:6443 --token jzs39q.nwcq6r957a3q9wsr \
	--discovery-token-ca-cert-hash sha256:fca657c6266564a69c2fbdb7152068bf777aa43b95a7c498a3ecbe1e4da5c2cd
```
```shell
# master node
# list of nodes
# master-node এর পাশে control-plane লিখা থাকবে  
kubectl get nodes

# verify
kubectl run nginx --image=nginx
kubectl get pod 
kubectl delete pod nginx
```

[Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
