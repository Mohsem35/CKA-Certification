### Example voting app

[Voting App GitHub Link](https://github.com/kodekloudhub/example-voting-app/tree/master/k8s-specifications)

**Goals:**

1. Deploy Containers
2. Enable Connectivity
3. External Access
   
<img width="750" alt="Screenshot 2023-12-28 at 11 12 07 PM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/77ef7c1a-c216-42f7-8b4f-677e9514a4e1">

**Steps:**

1. Deploy PODs

_How do you make one component accessible by another?_

_How do you make the Redis database accessible by the voting app?_

The right way to do it is to use a service. A service can be used to expose an application to other applications or users. So create a service for the Redis Pod. So that it can be accessed by the voting app and the worker app. We will call it a Redis service by name.

2. Create Services (ClusterIP)
    1. redis
    2. db

> যেহেতু internal services, তাই **`ClusterIP`** use করব 

3. Create Services (NodePort)
    1. voting-app
    2. result-app

result-app, voting-app **দুইটাই বাহির থেকে access করার প্রয়োজন পড়বে**। তারমানে port-mapping লাগবে to access these 2 applications. 

> _Note:_ যেইদিকেই port mapping করার ব্যাপার আসবে সেইখানে আমরা **`NodePort`** service use করব  


To summerize, we will be deploying 5 PODs in total and we have 4 services 


```shell
minikube service voting-service  --url

minikube service result-service  --url
```


> _NOTE_: আমরা pod create /apply এর পরিবর্তে deployment create করব। কিন্তু service এ কোন change আসবে না। pod হোক অথবা deployment হোক, service create করতেই হবে 


