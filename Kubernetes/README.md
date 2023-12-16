
### Kubernetes Architecture

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

**`Kubelet`**: Kubelet is the **agent** that runs on each node in the cluster. The agent is responsible for **making sure** that the containers are running on the nodes expected



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

List all the nodes part of the cluster

```shell
kubectl get nodes
```


### Docker vs Container-D

K8s was built to orchestrate Docker specifically in the beginning. So, Kubernetes was tightly coupled with Docker container. কিন্তু other containers runtime like `Rocket` এরা ও Kubernetes use করতে চাইল 

So, K8s introduced an interfac 