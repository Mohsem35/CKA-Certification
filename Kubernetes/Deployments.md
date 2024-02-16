### Deployments

A Deployment provides **declarative updates for `Pods` and `ReplicaSets`**.

like octopass, যার কাজ হচ্ছে অনেকগুলো pod manage করা 

You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.




When you upgrade your instances, you **do not want to upgrade all of them at once**. This may impact users accessing our applications, so you may want to upgrade them **one after the other**. And that kind of upgrade is known as **Rolling Updates**

যদি new containers গুলোতে update করার পরে কোন unexpected error পাই, আমরা rollback করতে চাইব 

Deployment which is a **kubernetes object** that comes higher in the hierarchy

The deployment provides us with capabilities to **upgrade the underlying instances seamlessly using rolling updates**, **undo changes**, and **pause and resume changes to deployments**


_How do we create a deployment_

The contents of the deployment-definition file are **exactly similar to the replicaset definition file**, except for the **kind**, which is now going to be **Deployment**

**`deployment-definition.yml`**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
          image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

Things to remember here

1. **`spec, metadata`** section পুরাটাই object 
2. **`matchLabels: front-end`** মানে এখানে deployment object, শুধুমাত্র front-end লেভেলিং করা pods গুলোকে handle করবে। 
3. ভিতরের **`spec`** এর ভিতরে থাকবে pod এর চেহারা ছবি কেমন হবে 
4. **`template`** এর ভিতরে পুরাটাই pod er জিনিষপত্র, আর template ছাড়া যা কিছু সব কিছু হচ্ছে deployment এর 
5. তারমানে `template` এর label এবং `selector` এর matchLabels একই হবে 



```shell
kubectl create -f deployment-definition.yml
kubectl get deployments

# will see a new replicaset with deployment name
kubectl get replicaset

# details
kubectl describe deployment myapp-deployment
```

**সবকিছু দেখতে চাইলে**

```
kubectl get all
```


### Updates & Rollback


##### Updates

Whenever we create a new deployment or upgrade the images in an existing deployment it triggers a Rollout. **A rollout is the process of gradually deploying or upgrading our application containers**. When you first create a deployment, it triggers a rollout. A new rollout creates a new Deployment revision. Let’s call it **revision 1**

<img width="600" alt="Screenshot 2023-12-23 at 7 24 11 PM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/f03a6952-d439-4ede-9d42-2a0ca66c26c9">


যখন আমি আমার deployment-configuration ফাইলে কোন কিছু change করে deploy দিব সেইটাই হচ্ছে updates


#### Rollback

updates হয়ে গেছে কিন্তু পরে চেক করে দেখলাম নতুন deployment এ ভুল এবং আমাদের আগের deployment এ ফিরে যেতে হবে। এইক্ষত্রে আমরা Rollback use করব 

<img width="600" alt="Screenshot 2023-12-23 at 7 25 33 PM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/a3445134-4410-4f98-a2b8-4367d7b6b663">


In the future **when the application is upgraded** – meaning when the container version is updated to a new one – a new rollout is triggered and a new deployment revision is created named **Revision 2**. This **helps us keep track of the changes made** to our deployment and **enables us to rollback to a previous version** of deployment if necessary.


We can see the **status of our rollout** by running the command: 

```shell
kubectl rollout status deployment/myapp-deployment
```

To see the **revisions and history of rollout** 

```shell
kubectl rollout history deployment/myapp-deployment
```

**`RollingUpdate`** is the **default Deployment Strategy**

To undo a change

```shell
kubectl rollout undo deployment/myapp-deployment
```

### Deployment Strategy

There are two types of deployment strategies. 

1. `Recreate Strategy`: One way to upgrade these to a newer version is to **destroy all of these and then create newer versions of application instances**. The problem is that during the period after the older versions are down and before any newer version is up, the application is down and inaccessible to users. This strategy is known as the **Recreate strategy**, and thankfully this is NOT the default deployment strategy.

2. `Rolling Strategy`: We take down the older version and bring up a newer version one by one. This way the application never goes down and the upgrade is seamless. It is called **RollingUpdate**.RollingUpdate is the **default Deployment Strategy**

<img width="700" alt="Screenshot 2023-12-23 at 7 20 41 PM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/028001a4-43fc-4388-b2d0-31486b8b4f89">


_How exactly DO you update your deployment?_ 

When I say update it could be different things such as **updating your application version** by updating the version of docker containers used, **updating their labels** or **updating the number of replicas** etc.


`deploymenr-definition.yaml` ফাইলে কোন key এর value change করে new করে `kubectl apply deployment` করলেই **`upgrade`** হয়ে যাবে  

> যেমন আগে `replicas: 3` ছিল এখন update করে `replicas: 4` করে `kubectl apply deployment` করব 

A new rollout is triggered and a new revision of the deployment is created.




### Summarize Deployment Commands

```shell
# create
kubectl create -f deployment-definition.yml --record

# get
kubectl get deployments

# update
kubectl apply -f deployment-definition.yml
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1

# status
kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/myapp-deployment

# details
kubectl describe deploymemt myapp-deployment

# edit
# edit করলে আলাদা করে deployment apply করার দরকার নাই 
kubectl edit deployment myapp-deployment --record

# delete 
kuebctl delete deployment myapp-deployment

# rollback
kubectl rollout undo deployment/myapp-deployment

# logs of a deployment
kubectl logs -f <deployment_name>
```

### Questions

Change the deployment strategy to `Recreate`

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      name: webapp
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        name: webapp
    spec:
      containers:
      - image: kodekloud/webapp-color:v2
        name: simple-webapp
        ports:
        - containerPort: 8080
          protocol: TCP
```

Note: strategy type `Recreate` হলে **rollingUpdate অংশটুকু বাদ যাবে**
