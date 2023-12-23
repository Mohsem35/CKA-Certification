**Creating a POD using a YAML configuration file**

### YAML in Kubernetes

K8s uses YAML files as inputs for the creation of objects such a PODs, replicas, deployment services, etc.

K8s definition file cobtains 4 top level fields

1. _apiVersion_
2. _kind_
3. _metedata_
4. _spec_

এর হল root/top level properties. Configuartion ফাইলে এই 4 টা fields থাকতেই হবে 

**`apiVersion`**: This is the version of the kubernetes API we’re using to create the object

| Kind  | Version |
| ------------- | ------------- |
| POD  | v1  |
| Service | v1  |
| ReplicaSet  | apps/v1  |
| Deployment | apps/v1  |


**`Kind`**: Kind refers to the type of object we are trying to create, which in this case happens to be a **POD**

আর কিছু values হতে পারে like **ReplicaSet**, **Deployment**, **Service**  

**`pod-definition.yml`**
```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end

spec:
  containers:
    - name: nginx-container
      image: nginx
```


**`metadata`**: The metadata is data about the object like its name, labels etc

এই field টা হবে dictionary form. **names(string value)** and **labels(dictionary)** are **children of metadata**

For example there are 100s of PODs running a front-end
application, and 100’s of them running a backend application or a database, it will be **DIFFICULT** for you to group these PODs once they are deployed. If you label them now as front-end, back-end or database, you will be able to filter the PODs based on this label at a later point in time.

**`spec`**: is a dictionary. spec এর under এ containers গুলো থাকবে। container is a **list/array**, কারণ এইখানে multiple container declare করতে হবে 

definition file declare করা হলে, we can run the following command and **Kubernetes creates the POD**

```shell
kubectl create -f pod-definition.yml
```

POD create হইলে, to see the **list of available PODs**

```shell
kubectl get pods
kubectl describe pod myapp-pod
```


### DEMO

`kubectl run` command এর পরিবর্তে আমরা YAML definition file use করব

Goal: is to create a YAML file with POD specification

```shell
vim pod.yml
```
```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels: 
    app: nginx
    tier: front-end

spec:
  containers:
  - name: nginx
    image: nginx
```
```shell
kubectl apply -d pod.yml
kubectl get pods
kubectl describe pod nginx

# Which nodes are these pods placed on?
kubectl get pods -o wide

# delete a pod
kubectl delete pod webapp
```

##### Questions

```shell
controlplane ~ ➜  kubectl get pods
NAME            READY   STATUS             RESTARTS   AGE
nginx           1/1     Running            0          11m
newpods-q2rx4   1/1     Running            0          11m
newpods-xh96h   1/1     Running            0          11m
newpods-jh4g4   1/1     Running            0          11m
webapp          1/2     ImagePullBackOff   0          4m40s
```

_What does the READY column in the output of the kubectl get pods command indicate?_

Running container in POD/total container in POD