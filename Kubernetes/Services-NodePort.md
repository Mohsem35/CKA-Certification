
We will discuss about Kubernetes Services

Kubernetes Services **enable communication between various components within and outside of the application**. Kubernetes Services helps us connect applications together with other applications or users

For example, our application has groups of PODs running various sections, such as a group for serving front-end load to users, another group running back-end processes, and a third group connecting to an external data source. **It is Services that enable connectivity between these groups of PODs**

Services enable the front-end application to be made available to users, it helps communication between back-end and front-end PODs, and helps in establishing connectivity to an external data source. 

Thus services enable **loose coupling** between microservices in our application.


কোন একটা K8s node এ ssh করে ঢুকে curl চালালে আমরা অবশ্যই response পাব, কিন্তু আমি চাচ্ছি আমার নিজের pc থেকে direct direct POD access করতে। এইক্ষেত্রে **Services** আসবে roleplay করতে 

The kubernetes **`service` is an object just like `PODs`, `Replicaset` or `Deployments`**

**Services Types**

1. _NodePort_
2. _ClusterIP_
3. _LoadBalancer_

### NodePort

Algorithm: Random

SessionAffinity: Yes


One of its use case is to **listen to a port on the Node and forward requests on that port to a port on the POD** running the web application. এই ধরনের service কে বলা হয় **NodePort service**. Because the service listens to a port on the Node and forwards requests to PODs. Node request accept করে এবং nodeport সেই request forward করে pod কে 


এখানে actually 3 টা port exists করতেছে 

- POD এর port (**TargetPort**)
- Service এর port itself (**Port**)

The **service** is in fact like a **virtual server inside the node**. Inside the cluster **service এর নিজের IP address থাকবে**। এই IP টাকে বলা হয় **`Cluster-IP of the service`**

- Node এর port (**Nodeport**)

NodePorts can only be in a **valid range** which is from 30000 to 32767


কিভাবে service declare করতে হয় ?

In `spec`, child `type` refers to the **type of service we are creating**. It could be **ClusterIP**, **NodePort**, or **LoadBalancer**

**`pod-definition.yaml`**
```yaml
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

**`service-definition.yaml`**

```yaml
apiVersion: v1
kind: Service
metadata: 
  name: myapp-service

spec:
  type: Nodeport
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  # copied from pod-definition file of `labels` to identify pod
  selector: 
    app: myapp
    type: front-end
```

- **যদি আমি targetPort না দেই**, তবে port আর targetPort একই হবে K8s ধরে নেয় 
- **যদি আমি NodePort না দেই**, তবে port range হবে 30000-32767 এর মধ্যে 


specifically কোন pod এর targetPort এর সাথে আমরা connect হব সেইটা declare করা হয় নাই, কারণ একটা node এ multiple pods থাকতে পারে 

এই issue solve করতে `labels` and `selector` use করতে হবে 

Solvence:

- **Pull the labels** from the **pod-definition file** and place it under the selector section

```shell
# create service
kubectl create -f service-definition.yaml

# servcie list
kubectl get services

# we can now access from our browser
curl http://192.168.1.2:30008
```



_Finally, lets look at what happens when the PODs are distributed across multiple nodee?_

In this case we have the web application on PODs on separate nodes in the cluster. When we create a service , without us having to do ANY kind of additional configuration, kubernetes creates a service that spans across all the nodes in the cluster and maps the target port to the SAME NodePort on all the nodes in the cluster. This way you can access your application using the IP of any node in the cluster and using the same port number which in this case is 30008.

To summarize – in ANY case weather it be a single pod in a single node, multiple pods on a single node, multiple pods on multiple nodes, the service is created exactly the same without you having to do any additional steps during the service creation.

When PODs are removed or added the service is automatically updated making it highly flexible and adaptive. Once created you won’t typically have to make any additional configuration changes.