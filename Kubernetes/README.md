
_Contents:_

1. [Kubernetes Architecture](#kubernetes-architecture)
    - [Nodes](#nodes)
    - [Cluster](#cluster)
    - [Master](#master)
    - [K8s Components](#k8s-components)
    - [Master vs Worker Nodes](#master-vs-worker-nodes)

2. [Docker vs containerD](#docker-vs-containerd)
    - [CRI](#cri)


### Kubernetes Architecture


![components-of-kubernetes](https://github.com/Mohsem35/CKA-Certification/assets/58659448/df70a1ab-e03d-445b-8227-3a0693bfa490)



#### Nodes

A **`node`** is a machine, physical or virtual on which Kubernetes is installed. A node is **worker machine** and that is where **containers will be launched** by K8s. It was also known as minions in the past

_Q: What if the node on which your application is running fails?_

So we need to have more than one nodes



#### Cluster 

A Cluster is a set of **nodes grouped together**. This way even if one node fails you have your application still accessable form other nodes. Moreover having multiple nodes helps in **sharing load as well**. 

_Now we have cluster and who is responsible for managing ths cluster? Where is the information about members of the cluster? How are the nodes monitored? When a node fails how do you move the workload of the field node to another worker node?_ 

#### Master

Master is another node with Kubernetes installed in it and is configured as a Master.

The master **watches over the nodes** in the cluster and is responsible for the **actual orchestration** of container on the worker nodes.


When we install K8s, we are actually installing the following components.

#### K8s Components

**`API Server`**: acts as the **front-end for Kubernetes**. The users, management devices, command line interfaces, all talk to the API server to interact with Kubernetes cluster.

**`etcd key store`**: `etcd` is a distributed reliable **key-value store** used by Kubernetes to store all data used to manage the cluster.

যখন you have multiple nodes and multiple masters in your cluster, `etcd` **stores all that information on all the nodes** in the cluster in  a **distributed manner**.

`etcd` is responsible for implementing locks within the cluster to ensure that there are no conflicts between the Masters.


**`Scheduler`**: The scheduler is responsible for **distributing work** or containers across multiple nodes. It looks for newly created containers and assigns them to nodes.

**`Controller`**: The controllers are the **brain** behind orchestration. They are responsible for noticing and responding when nodes, containers or end points goes down. The controllers **make decisions** to **bring up new containers** in such cases. 

**`Container Runtime`**: is the underlying software that is **used to run containers**. In our case, it happens to be `Docker`. 

**`Kubelet`**: Kubelet is the **agent** that runs on each node in the cluster. The agent is responsible for **making sure that the containers are running** on the nodes expected



#### Master vs Worker Nodes

_How does the one server become server and the other the slave?_

Worker node হল সেইটা যেইটাতে containers গুলো hosted করা থাকে like Docker Containers. and একটা system তে docker container run করতে হলে we need to install `container runtime`.

এইদিকে container runtime হল **`Docker`**. There are other container runtime alternatives available such as `rkt` or `CRI-O`

**Master** server এর মধ্যে **kube API server** আছে, এবং এই কারণেই node master হয়. যত রকমের key-value information আছে **`etcd`** তে সেইটা Master node তে থাকে। Master node তে **`controller`** and **`scheduler`** ও থাকে 

Similary, The **Worker** nodes have the **kubelet agent** that is responsible for interacting with a master to provide health information of the worker node and carry out actions requested by the Master on the Worker nodes.

**`kubectl`** tool is used to **deploy and manage applications** on a Kubernetes cluster.
- to get cluster infromation
- to get the status of other nodes in the cluster


Command is used to **deploy an application on the cluster**
```shell
kubectl run hello-minikube
``` 

**View information of the cluster**

```shell
kubectl cluster-inf
```

**List all the nodes** part of the cluster

```shell
kubectl get nodes
```

**Version check** 

```shell
kubectl version
```

```shell
kubectl get nodes -o wide
```

### Docker vs containerD

K8s was built to orchestrate Docker specifically in the beginning. So, Kubernetes was tightly coupled with Docker container. কিন্তু other containers runtime like `Rocket` এরা ও Kubernetes use করতে চাইল 

#### CRI

So, K8s introduced an interface called **`Container Runtime Interface(CRI)`**. So, CRI allowed any vendor to work as a container runtime for Kubernetes as long as they adhere to the **OCI standards**.

OCI stands for **`Open Container Initiative`** and consists of image spec and runtime spec.

`imagespec` means the specification on how an image should be built.

`runtimespec` defined the standards on how any container runtime should be developed.

এই **দুইটা concept** মনে রেখে যে কেউ **container runtime build করতে পারে**, and that can be used by anybody to work with Kubernetes. So, that was the idea

> Docker was not built to support the CRI standards, কারণ docker built হইছে CRI introduced হওয়ার আগে 

Kubernetes introduced **`dockershim`** which is a hacky but temporary way to continue to support Docker outside of the container runtime interface. যেইদিকে most container runtimes work through `CRI`, Docker `CRI` ছাড়াই কাজ করে। **Docker** শুধুমাত্র একটা container runtime ই নয়, it **consists of multiple tools** that are put together.  

- CLI
- API
- Build
- Volumes
- Auth
- Security
- Container Runtime (runc)
- Conatainer-D (Daemon that managed runc)

`containerD` is directly **compatiable with CRI** and can work directly with Kubernetes as all other runtimes.

সুতরাং, containerD কে seperate runtime হিসেবে use করা যাইতে পারে from Docker

`dockershim` remove করা হইছে updated docker version গুলোতে। 

Docker was removed as a supported runtime from K8s.

containerD is a seperate project now. containerD seeprate ভাবে install করা যাবে without having to install Docker itself. 

_When Docker is not installed, then how do you run containers with just containerD?_

containerD install করলে **`CTR`** command line tool থাকে। This tool is **solely made for debugging** and containerD and is **not very user-friendly**. 

`ctr` দিয়ে basic container related tasks গুলো perform করা যায় 

`ctr`এর better alternative হচ্ছে `nerd control tool`

**CLI - nerdctl**

- nerdctl provides a Docker-like CLI for containerD 
- nerdctl supports docker compose
- nerdctl supports newest features in containerD
    - Encrypted container images
    - Lazy Pulling
    - P2P image distribution
    - Image signing and verifying 
    - Namespaces in Kubernetes

So, nerd control tool একটি command line tool যেইটা very similar to Docker. Docker যা যা support করে তার বেশিরভাগ এইটা support করে।  

> `CTR`, `CLI - nerdctl` built by containerD community for containerD

Kubernetes community নিজে যেইটা বানাইছে সেইটা হল **`crictl(ক্রাই কন্ট্রোল)`**. Mainly used to interact with CRI-compitable runtime. This us mainly used foe debugging purpose

আগে আমরা যেইদিকে `docker` command use করতাম, এখন সেইদিকে `crictl` command use করব 
