Why Orchestrate ?

Container Orchestration

It's a solution that consists of a set of **tools and scripts** that can help containers in a production environment.

With **`Docker Swarm`**, we could combine multiple Docker machines together into a single cluster. Docker Swarm will take care of distributing our services or our application instances into seperate hosts for HA and load balancing across different systems and hardware.


Docker Swarm set up করার জন্য, we must have **multiple hosts with Docker installed** on it.

- Designate one host to be the **manager/master**
- Others as **worker/slave**


Run the following command in swarm-manager node

```shell
# swarm manager node
docker swarm init
```
Copy the **token number** from the manager node, then run following command to worker nodes

```shell
# worker nodes
docker swarm join --token <token>
```

The key component of swarm Orchestration is the **Docker servcie**. Docker service create করে multiple instances of the application across worker nodes in my swarm cluster.

`Docker service` always create করতে হবে **manager node** তে 

`Docker service` command is similar to `Docker run` command। `docker run` command তে আমরা যেই options গুলো use করতাম, এইদিকে ও same use করা যাবে 

```shell
# manager node
docker service create --replicas=3 <my_image_name>
docker service create --replicas=3 -p 8080:80 <my_image_name>
```

