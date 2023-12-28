
**ClusterIP is the default service in Kubernetes**

A full stack web application typically has different kinds of PODs hosting different parts of an application. You may have a number of **PODs running a front-end web server**, another set of **PODs running a backend server**, a set of PODs running a key- value store like **Redis**, another set of PODs running a persistent database like **MySQL** etc. The web front-end servers need to connect to the backend-workers and the backend-workers need to connect to database as well as the redis services. 

_Q: So what IS the right way to establish connectivity between these PODs?_

The **PODs all have an IP address assigned** to them as we can see on the screen. **But these IPs are not static**, these PODs can go down anytime and new PODs are created all the time – and so you **CANNOT rely on these IP addresses** for internal communication within the application. 

_Q: Also what if the first front-end POD at 10.244.0.3 need to connect to a backend service?_

_Q: Which of the 3 would it go to and who makes that decision?_

A kubernetes **`service`** can help us 
- **group these PODs together** and 
- **provide a single interface to access the PODs in a group**. 

For example a service created for the backend PODs will help group all the backend PODs together and provide a single interface for other PODs to access this service. Grouping করার পরে **requests are forwarded to one of the PODs under the service randomly**.


একটা **additional service create করব** for Redis and backend PODs গুলো **Redis system কে access করবে through that service**. This **enables us to easily and effectively deploy a microservices based application on kubernetes cluster**. Each layer can now scale or move as required without impacting communication between the various services. 


Each service gets an IP and name assigned to it inside the cluster and that is the name that should be used by other PODs to access the service. This type of service is known as ClusterIP.

মানে সহজ হিসাব হচ্ছে আমরা সবগুলো PODs কে একত্রে করে একটা cluster করব এবং সেই cluster টা একটা service হিসেবে count করব, যার একটা নিজস্ব IP and Name থাকবে। সেই নাম ধরেই অন্য PODs গুলো request করবে। 

এই service টাকেই বলা হচ্ছে **ClusterIP** 



**`service-definition.yaml`**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end

spec:
  type: ClusterIP
  ports: 
    - targetPort: 80
      port: 80

  # copied from pod-definition file of `label` section  
  selector:
    app: myapp
    type: backend  
```

```shell
kubectl create -f service-definition.yaml

# service list
kubectl get service
```


#### Questions

_What is the targetPort configured on the kubernetes service?_

_How many labels are configured on the kubernetes service?_

```
kubectl describe service
```

```shell
controlplane ~ ➜  kubectl describe service
Name:              kubernetes
Namespace:         default
Labels:            component=apiserver
                   provider=kubernetes
Annotations:       <none>
Selector:          <none>
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.0.1
IPs:               10.43.0.1
Port:              https  443/TCP
TargetPort:        6443/TCP
Endpoints:         192.11.11.9:6443
Session Affinity:  None
Events:            <none>

```