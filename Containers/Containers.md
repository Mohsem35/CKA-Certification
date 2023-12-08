[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

```shell
sudo docker version
```
docker and docker-hub দুইটা completely different জিনিষ 

#### Basic Commands

```shell
# start a container
docker run nginx
```
**Run an instance of the nginx application** on the Docker host. If the **image is no present** on the host, it will go out to **Docker hub and pull the image down**. But, this is only done for the **first time**. 

```shell
# list containers
docker ps -a
```
```shell
# stop a container
docker stop <container_id>
docker stop <container_name>
```

```shell
# remove a container
docker rm <container_id>
docker rm <container_name>
```

```shell
# images list
docker images
```

```shell
# remove image
docker rmi <image_name>

# ! Delete all dependent containers to remove image
```

```shell
# download an image
docker pull <image_name>
```

Containers are not meant to host an operating system. **Containers** are meant to **run a specific task or process** such as to host an instance of a web server or application server or a database or simply to carry some kind of computational or analysis task. **Once the task is complete, the container exists**. A **container only lives as long as the process inside it is alive**. If the web service inside the container is stopped or crashed, then the container exists. 

This is why when you run a container from an **Ubuntu** image, it **stops immediately**. Because Ubuntu is just an image of operating system that is used as the base image for other applications. There is no process or application running in Ubuntu by default.


```shell
# execute command inside container
docker exec <container_name> cat /etc/hosts
```
```shell
# attach and detach
docker run -d kodekloud/simple-webapp
a043d40f85fefa414254e4775f9336ea59e19e5cf597af5c554e0a35a1631118

docker attach a043d
```
Docker container in the detached mode by providing the **`d`** option. This will run the docker container in the **background mode**, and **you will back to your prompt immediately**. The container will continue to work in the backend.

Docker Hub থেকে offical images(nginx, centos, redis...) গুলো pull হয় করলে **`library/image_name`** repository থেকে pull হয় otherwise unioffical হইলে pull হবে **`user_id/image_name`** format থেকে 

```shell
docker run -it centos bash
```

Docker ran the CentOS container, which logged me in directly to the container since we specified the **`-it`** options.

Command টা এইভাবে run করা হইল, যাতে CentOS container টা **exit mood** এ **চলে যাতে না পারে** 



```shell
# To delete all containers including its volumes use
docker rm -vf $(docker ps -aq)
```
```shell
# To delete all the images
docker rmi -f $(docker images -aq)
```

```shell
# Use this to delete everything:
docker system prune -a --volumes

Remove all unused containers, volumes, networks and images
WARNING! This will remove:
    - all stopped containers
    - all networks not used by at least one container
    - all volumes not used by at least one container
    - all images without at least one container associated to them
    - all build cache
```
##### Docker Tag

কোন software package যদি latest install করতে না চাই, তবে image pull করার সময় package version declare করে দিতে হবে । এইটারেই **`Tag`** বলে 

```shell
docker run redis:4.0
```
এখানে 4.0 হল Tag  

##### Docker STDIN

**`i`** parameter is used for **interactive mode**

**`t`** stands for **pseudo terminal**

```shell
docker run -it kodekloud/simple-prompt-docker
```
With the combination of `i` and `t`, we are now **attached to the terminal** as well as in an **interactive mode** on the **container**

##### Docker PORT Mapping 

We can map port 80 of local host to port 5000 on the docker container using the **`-p`** parameter in command.

**Internal IP** is only **accessible** within the **docker host**. So if you open a browser from within the docker host, you can access that service

```shell
# docker run -p <host_ip>:<container_ip> kodekloud/webapp
docker run -p 80:5000 kodekloud/webapp
```

##### Docker VOLUME Mapping

Data persistent in docker container. Docker container has its **own isolated file system**, and any changes to any files happen within within the container. 

```shell
docker run -v /opt/datadir:/var/lib/mysql mysql
```

We would want to map a directory outside the container on the Docker host to a directory inside the container. It will implicitly mount the external directory to a folder inside the docker container

##### INSPECT Container

```shell
docker inspect <container_name>
```

##### Container LOGS

```shell
docker logs <container_name>
```
