
### Docker Compose

If we need to set up a complex application running multiple services, a better way to do it is to use `Docker Compose`. With the compose file we cloud create a configuration file in `.yml` format called `Docker-compose.yml`.    


#### Sample application - voting application

When a container is dependent on another container, we have to use **container links**

**`link`** is a **command line option**, which can be used to link two containers together.

```shell
# docker run -d --name=vote -p 5000:80 --link <container_name>:<host_app_name> voting-app
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app
```

এখন যদি আমি voting-app container এর ঢুকে, `cat /etc/hosts` command run করি তবে IP সহ redis এর একটা entry দেখতে পাব 

`Docker-compose` by default **bridged network** use করে এবং সবগুলো container কে একটা bridged network এর under এ আনে 

Most popular Docker compose usage eaxample : [example-voting-app](https://github.com/dockersamples/example-voting-app)


Testing server IP: 172.16.6.57

```shell
# voter app
git clone https://github.com/dockersamples/example-voting-app.git
cd example-voting-app/vote/
docker build . -t voting-app
docker images
docker run -p 5000:80 voting-app
crtl + c
```
```shell
# redis app
docker run -d --name=redis redis
docker run -p 5000:80 --link redis:redis voting-app
```
No exit, app will be running

```shell
# db
docker run -d -i --name db -p 5432:5432 -e POSTGRES_HOST_AUTH_METHOD=trust postgres:12
```
but exited। এইটা exited হবে যদি `POSTGRES_PASSWORD` use না করি 

```shell
# worker app
cd example-voting-app/worker/
docker build . -t worker-app
docker run --link redis:redis --link db:db worker-app
```

```shell
# result app
cd example-voting-app/result
docker build . -t result-app
docker run -p 5001:80 --link db:db result-app
```

কিন্তু আমরা এইভাবে বারবার একটা একটা করে container up না করে, `docker-compose.yml` ফাইলে সবকিছু declare করে দিতে পারি। 

> `Docker-compose` file version 1.0 অনেক কিছু support করে না, তাই **`version 3.0`** করব আমরা 

**docker-compose ফাইল লিখার জন্য** এই documentation টা **follow** করব: [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/compose-file-v3/)

[docker compose](https://docs.docker.com/engine/reference/commandline/compose/)

```shell
$ docker images
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
result-app          latest    5f98571a19fa   8 hours ago     224MB
worker-app          latest    23c65001c11f   18 hours ago    194MB
voting-app          latest    cdc659974494   18 hours ago    154MB
postgres            12        cf2ffc3ab9f7   4 days ago      412MB
redis               latest    76506809a39f   7 days ago      138MB
```

```yaml
version: "3"
services:
  redis: 
    image: redis

  db:
    image:  postgres:12
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres

  vote:
    image: voting-app
    ports:
      - 5000:80

  worker:
    image: worker-app

  result:
    image: result-app
    ports:
      - 5001:80
```

এইদিকে `docker-compose.yml` ফাইল বেশি বড় হয় নাই, তার কারণ আমরা আগেই Dockerfile অর্থাৎ image গুলো build করে নিয়েছি । এইদিকে শুধুমাত্র image name ধরে container গুলো up করেছি 

```shell
docker-compose up
```