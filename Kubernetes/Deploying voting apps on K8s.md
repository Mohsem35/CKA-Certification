
### Example voting app

**Goals:**

1. Deploy Containers
2. Enable Connectivity
3. External Access

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
