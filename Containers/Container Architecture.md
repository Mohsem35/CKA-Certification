### Docker Engine
When you install a Docker on a Linux host, we are actually installing 3 different components.


| Docker Engine | 
| ------------- | 
| Docker CLI  | 
| REST API  |
| Docker Deamon  |

`Docker deamon` is a background process that manages Docker objects as the **images**, **containers**, **volumes**, and **networks**.

`Docker REST API` server is the API interface that programs can use to **talk to the daemon and provide instructions**. We can create our own tools using REST API.

`Docker CLI` is nothing but the command line interface. It **uses the REST API to interact   with the Docker daemon**.

> Docker CLI need not necessarily be on the same host

**Let's understand how exactly are applications containerized in Docker** 


Docker uses namespaces to isolate workspace, process IDs, network, interprocess communication, mouts, and UNIX timesharing systems are created in their own namespace thereby providing isolation between containers.

#### Process ID Namespaces

যখন Linux system boots up, it starts with just one process with a process ID of 1. এইটা root process and kicks off all the other processes in the system. যখন system boots up completely, there are handful of processes running.

Process IDs are unique, ২ টা process ID কখনও same হতে পারে না 

Container is basically like a child system within the current system. Child system needs to think, it is an independent system on its own, and it has its own set of processes originating from a root process with a process ID of 1. 

but we know that, there is no hardware isolation between containers and undelying host.

So, processes runnning inside the container are in fact processes running on the underlying host. যেহেতু ২ তা process এর same process ID 1 থাকতে পারবে না, এইক্ষেত্রে namespaces come into play. 

With process ID namespaces, each process can have multiple process IDs associated with it.

For example, container এর মধ্যে যখন কোন process start হয়, it's actually just another set of processes on the base Linux system and it gets the next available process ID. Container thinks that it has its own root process tree, so it is an indepedent system.

Containers and host shared the same system resources such as CPU and memory. Host machine and container কে কত resource use করবে, By default কোন restrictions দেয়া থাকে না এবং container host machiner এর resource full utilization করতে পারে। 

Docker use `cgroup` or `control groups` to restrict the amount of hardware resources allocated to each container. এজন্য আমাদের **`--cpus`** command option use করতে হবে container run করার সময়

```shell    
# container doesn't take up more than 50% of the host CPU at any given time
docker run --cpu=.5 ubuntu

# 100m এর বেশি memory use করবে না 
docker run --memeory=100m ubuntu
```