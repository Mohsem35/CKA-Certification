
Controllers are the brain behind Kubernetes

They are processes that **monitor kubernetes objects** and respond accordingly.

_So what is a replica and why do we need a replication controller?_

1. High availability
2. Load balancing & Scaling


It’s important to note that there are two similar terms. **Replication Controller** and **Replica Set**. Both have the same purpose but they are not the same. **Replication Controller is the older technology** that is being replaced by Replica Set. Replica set is the new recommended way to setup replication

#### ReplicationController

**`rc-definition.yaml`**

Most crucial field of replication definition file is **`spec`** field. For any kubernetes definition file, the spec section defines **what’s inside the object we are creating**. In this case we know that the replication controller **creates multiple instances of a POD**.

**এই ফাইলে ২ টা spec section আছে**, একটা ReplicationController এর জন্য আরেকটা POD এর জন্য 

`ReplicautionController` parent and `POD` child

```yaml
apiVersion: v1
kind: ReplicationController
# ReplicationController metadata
metadata:
  name: myapp-rc
  labels: 
    app: myapp
    type: front-end

spec: 
  - template:
    # POD metadata, copied from POD difinition file  
      metadata:
        name: myapp-pod
        labels:
          app: myapp
          type: front-end
        spec:
          containers:
          - name: nginx-container
            image: nginx
  - replicas: 3
```

We create a template section under spec to provide a POD template to be used by the replication controller to create replicas.

apiVersion and kind **বাদ দিয়ে** বাকি অংশটুকু template section এ ঢুকিয়ে দিব 

```shell
kubectl create -f rc-definition.yaml
```
এই command টা execute করলে প্রথমে POD create হবে, তারপর replication হবে 


```shell
# list of created replication controllers
kubectl get replicationcontroller

# list of pods
kubectl get pods
```

#### ReplicaSet

**Similar to ReplicationController**

_Major difference_: Replica set **requires a `selector` definition**. The selector section helps the replicaset **identify what pods fall under it**.

_But why would you have to specify what PODs fall under it, if you have provided the contents of the pod-definition file itself in the template?_ 

BECAUSE, replica set can **ALSO manage pods** that **were not created** as part of the replicaset creation.

Say for example, there were **pods created BEFORE the creation of the ReplicaSet** that **match the labels specified in the selector**, the replica set will also **take THOSE pods into consideration** when creating the replicas

**`replicaset-definition.yaml`**

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels: 
    app: myapp
    type: front-end

spec: 
  - template:
      metadata:
        name: myapp-pod
        labels:
          app: myapp
          type: front-end
        spec:
          containers:
          - name: nginx-container
            image: nginx
  - replicas: 3
  - selector: 
      matchLabels:
        type: front-end
```

```shell
# create replicaset
kubectl create -f replicaset-definition.yaml

# list of replicaset
kubectl get replicaset
kubectl get pods
```


### Labels and Selectors


যদি আগে থেকেই কোন pod create করে থাকি তবে replicaset দিয়ে সেই **existing pods গুলো monitor করা** যাবে। In case **যদি pod create করা না হয়ে থাকে**, তবে **replicaset সেইটা আমাদের জন্য create করে দিবে** 

The role of the replicaset is to monitor the pods and if any of them were to **fail, deploy new ones**. The replica set is in FACT a process that monitors the pods.


_How does the replicaset KNOW what pods to monitor. There could be 100s of other PODs in the cluster running different application_

এইদিকে আসবে labeling এর efficiency

**POD creation এর সময় আমরা যেই label use করছিলাম**, ReplicaSet creation এর সময় আমরা `selector -> matchLabels -> type` **ঠিক সেই label টাই use করব**. This way the replicaset knows **which pods to monitor**


```yml
# replicaset-definition.yaml
  - selector: 
      matchLabels:
        type: front-end

# pod-definition.yaml
metadata:
  name: myapp-pod
  labels:
    type: front-end
```

### Scale ReplicaSet

We started with 3 replicas and in the future we decide to scale to 6. 

_How do we update our replicaset to scale to 6 replicas_

1st Approach: **Update the number of replicas in the definition file to 6**. Then run the `kubectl replace` command specifying the same file using the `–f` parameter and that will update the replicaset to have 6 replicas.

```yaml
# in replicaset-definition.yaml
  - replicas: 6
```
```
kubectl replace -f replicaset-definition.yaml
```

2nd Approach: Run the **`kubectl scale` command**

```shell
kubectl scale --replicas=6 -f replicaset-definition.yaml
```

or
```shell
#                           type   name of metadata
kubectl scale --replicas=6 replicaset myapp-replicaset
```


#### Commands

```shell
kubectl create -f replicaset-definition.yaml
kubectl get replicaset

kubectl delete replicaset myapp-replicaset  # also delets all underlying pods
kubectl replace -f replicaset-definition.yaml

# without modify the file
kubectl scale --replicas=6 -f replicaset-definition.yaml
```