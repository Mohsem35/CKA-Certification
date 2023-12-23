
### POD

Kubernetes doesn't deploy containers directly on the worker nodes. The **containers are encapsulated** into a Kubernetes object known as **`Pods`**.

A Pod is a **single instance of an application**. A Pod is the **basic and smallest object** that you can create in Kubernetes.

_Q: What if the number of users accessing our application increase and we need to scale our application?_

আমাদের additional instance of the application add করতে হবে, যাতে করে load share করা যায় instances গুলোর মধ্যে 

_Q: Now wherer would you spin up additional instances?_

New `Pod` add করতে হবে with a new instance of the same application within the **`Node`**। মানে resource scaling করতে হবে `Pod` add করার through তে। 

`Pods` usually have **one-to-one relationship with containers** running our application.
- To `scale up`, new Pods add করব
- To `scale down`, delete existing Pods

Scaling করতে(application) আমরা existing `Pod` এর মধ্যে additional containers add করি না। Scaling করার ব্যাপারটা Pod এর সাথে related, container এর সাথে না 


### Multi-Container PODs

_Q: Are we restricted to having a single container in a single Pod?_

No. A single Pod can have multiple containers except for that fact that multiple container গুলো same kind নয়। 

We want the helper containers to live alongside our application container. এইসব ক্ষেত্রে আমরা containers গুলোকে same Pod এর মধ্যে রাখতে পারি। 

**`Helper Containers:`** যেই container গুলো supporting role হিসেবে কাজ করে for our main application like processing and fetching data from elsewhere.

**একটা Pod এর under এ multiple containers** can communicate with each other directly by referring to each other as localhost যেহেতু they share the **same network space** সাথে **same storage spcae** ও ব্যবহার করে। 


### How to Deploy a Pods

```shell
kubectl run nginx --image nginx
```

`Kubectl` deploys a Docker container by creating a Pod. It first **creats a Pod automatically** and **deploys an instance of the nginx Docker image**.

**List of Pods** in cluster

```shell
kubectl get pods
```
Get **more information related to the Pod**

```shell
kubectl describe pod <pod_name>
```

Check the status of the Pods

```shell
kubectl get pods -o wide
```

### Demo - Pods

We are going to deploy a Pod in our minikube cluster


A note about creating pods using `kubectl run`

To **create a pod** from the command line, use the command:

**Create an NGINX Pod**

```shell
kubectl run nginx --image=nginx
```

As of version 1.18, kubectl run (without any arguments such as --generator ) will create a pod instead of a deployment.

To **create a deployment** using imperative command, use `kubectl create`:

```shell
kubectl create deployment nginx --image=nginx
```


