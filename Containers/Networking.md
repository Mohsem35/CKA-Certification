When we install Docker, it creats 3 networks automatically.
1. Bridge
2. None
3. Host

**`Bridge`** is the **default network** a container gets attached to and containers get an internal IP addresses usually in the range `172.17` series. Container গুলো একে অপরের সাথে communication করতে পারে এই internal IP ধরে। 

To access these container from Outside world, map the ports of these containers to ports on the docker host

Bridge network is a private internal network created by Docker on the host.

যদি অন্য কোন network এর সাথে container associate করতে চাই, তবে `network` parameter oprion declare করে দিতে হবে container run করার সময়

```shell
docker run ubuntu --network=host
```

Another way to access the containers externally is to associate the container to the **`host network`**. This takes out any network isolation between the docker host and the docker container. 

এইদিকে আলাদা করে port mapping করার প্রয়োজন নাই Docker host machine এর সাথে। Direct host machine এর IP use করে container and accessable from outside world as the container uses the host's network.

Meaning, একই container বার বার use করতে পারব না same port দিয়ে 

`None network` এর কোন access নাই outside world এর সাথে। They run in an isolated network


### User-defined Networks

By default, Docker only creats **one internal bridge network**

যদি নিজেরা internal netwrok create করতে চাই, তাহলে command হবে 

```shell
docker network create \ 
    --driver bridge \
    --subnet 182.18.0.0/16 
    custom-isolated-network
```
```shell
docker network ls
```

#### Inspect Network

```shell
docker inspect <container_id>
```

#### Embedded DNS

Container can reach each other using their names.

All containers in a docker host can resolve each other with the name of the container. **Docker এর built-in DNS সার্ভার আছে**, which helps the containers to **resolve each other using the container name**.

The **bulit-in DNS server** always runs at address `127.0.0.11`


_Q. So how does Docker implement networking? What's the technology behind it? How are the containers isolated within the host ?_ 

1. Docker **Network-Namespace** use করে যেইটা create করে **seperate namespace for each container**.
2.It then uses Virtual Ethernet pairs to connect container together 

_Q1: What is the subnet configured on bridge network?_
```shell
docker network inspect bridge
```
_Q2: Create a new network named `wp-mysql-network` using the bridge driver. Allocate `subnet 182.18.0.1/24`. Configure `Gateway 182.18.0.1`_

```shell
docker network create --driver bridge --subnet 182.18.0.1/24 --gateway 182.18.0.1 wp-mysql-network
```

_Q3: Deploy a mysql database using the `mysql:5.6` image and name it `mysql-db`. Attach it to the newly created network `wp-mysql-network`. Set the database password to use `db_pass123`. The environment variable to set is `MYSQL_ROOT_PASSWORD`_

```shell
docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -d --network=wp-mysql-network mysql:5.6
```


_Q4: Deploy a web application named webapp using the `kodekloud/simple-webapp-mysql` image. Expose the port to 38080 on the host._

The application makes use of two environment variable:
1. `DB_Host` with the value `mysql-db`.
2. `DB_Password` with the value `db_pass123`.
3. Make sure to attach it to the newly created network called `wp-mysql-network`.

Also make sure to `link the MySQL and the webapp container`.

```shell
docker run --name webapp --network=wp-mysql-network \
    -e DB_Host=mysql-db -e DB_Password=db_pass123 \
    --link mysql-db:mysql-db -p 38080:48 \
    -d kodekloud/simple-webapp-mysql
```
**You don't get what you wish for. You get what you work for**

