_Q: How to create your own image ?_ 

It clould either be because you can't find  a component or a service that you want to use as part of your application on Docker Hub or you and your team decided that the application you are developing will be dockerized for ease of shipping and deployment.


### DOCKERFILE

_Q: How to create my own image ?_

1. OS - Ubuntu
2. Update apt repo
3. Install dependencies using **`apt`**
4. Install Python dependencies using **`pip`**
5. Copy source code to `/opt` directory
6. Run the web server using **`flask`** command

The project is hosted in github. You can look the project
[simple-webapp-flask application](https://github.com/mmumshad/simple-webapp-flask)

```Dockerfile

# INSTRUCTION Argument
FROM Ubuntu:20.04

RUN apt-get -y update && \
    apt-get -y install python3 &&
    apt-get install -y python3-pip

RUN pip install flask flask-mysql

COPY app.py /opt/source-code

WORKDIR /opt

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

```shell
docker build Dockerfile -t <image_name> 
docker run <image_tagname>
```
This will create an image locally on your system. 



To make available on the **public Docker Hub registry**

```shell
docker push <user_id>/<image_tagname>
```

Dockerfile is a text file written in a specific format that Docker can understand. It's in an instruction and argument format. 

In Dockerfile, everything in **CAPS is an instruction** like `FROM`, `RUN`, `COPY`, `ENTRYPOINT` are all instructions. Each of these instructs Docker to perform  a specific action while creating the image.

Everything on the right is an argument to those instructions

**`FROM`**: Dockerfile Start from a base OS or another image. All **Dockerfile must start** with a `FROM` instruction.

**`RUN`**: Instructs Docker to run particular **linux commands** on those base images

**`COPY`**: Copies files from the local system repo onto the Docker image. In above case, the **source code** of our application is in the current directory and we will be copying it over to the location `/opt/source-code` directory inside the Docker image.

**`ENTRYPOINT`**: Allows us to specify a **command that will be run** when the image is run **as a container**

When Docker builds the images, it builds these in a **layered architecture**. Each line of instruction creates a new layer in the docker image with just the changes from the previous layer.

**Image to Container**, প্রতিটা layer কি কি change হইছে সেইটা দেখতে হলে
```shell
docker history <image_name>
```

All the layers built are cached by Docker। কোন একটা particular layer fail করলে, যদি আবার docker build করি তবে Docker will re-use the previous layers from cache and continue to build the remaining layers. The same is true if we add new additional steps/layers in the Dockerfile. So we don't have to wait for a docker to rebuild the entire image each time

এইটা helpful যখন source-code বার বার update করা হয়  


### ENV variables

```shell
# docker run -e <env_variable_name>=<value> <image_name>
docker run -e APP_COLOR=blue simple-webapp-color
```

How do you find the environment varibale set on a container that's already running ?

Under the `Config` section, we will find the list of **all environment variables** enlisted in the container
```shell
docker inspect <container_name>
```
```
"Config": { "Env": [
    'APP_COLOR=blue"
    "LANG=C.UTF-8",
    "GPG_KEY=0D96DF4D4110E5C43FBFB17F2D347EA6AA65421D",
    "PYTHON_VERSION=3.6.6",
    "PYTHON_PIP_VERSION=18.1"
]
}
```
_Q1: Run a container named `blue-app` using image `kodekloud/simple-webapp` and set the environment variable `APP_COLOR` to `blue`. Make the application available on port `38282` on the host. The application listens on port `8080`_

```shell
docker run --name blue-app  -p 38282:8080 -e APP_COLOR=blue kodekloud/simple-webapp
```

_Q2: Deploy a `mysql` database using the`mysql` image and name it `mysql-db`. Set the database password to use `db_pass123`. Lookup the mysql image on Docker Hub and identify the correct environment variable to use for setting the root password_

```shell
docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -d mysql:8.2
```


### CMD vs ENTRYPOINT

CMD defines the program that will run within the container 

```shell
CMD ["nginx"]
```
```shell
CMD sleep 5
CMD ["sleep", "5"]
```
এই `CMD` তে আমরা command **`shell`** or **`json`** array format এ declare করতে পারি

Remember, when you specify it in a `JSON array format`, the **first element in the array should be the executable**. The command and parameters should be **seperate elements** in the list by comma.

**CMD হচ্ছে hard-coded approach**

Append a command to the docker run command, and that way it overrides the default command specified within the image.

ENTRYPOINT তে আমরা শুধু **argument(value) টা pass করব** while executing the run command

ENTRYPOINT instruction is like command instruction as you can specify the program that will be run when the container starts.

```
ENTRYPOINT ["sleep"]
```
```shell
docker run ubuntu-sleeper 10
```

২ টাকেই একসাথে use করতে চাইলে এবং must JSON array format এ declare করতে হবে 

```shell
ENTRYPOINT ["sleep"]

CMD ["5"]
```
Fina command run at startup will be: `ENTRYPOINT CMD`

> আগে ENTRYPOINT execute হবে, তারপর CMD execute হবে 

**override** করতে চাইলে 
```shell
docker run --entrypoint sleep2.0 ubuntu-sleeper 10

# output
Fina command run at startup: sleep2.0 10
```


### Docker Registry

The registry is where all the images are stored

Many cloud service providers such as AWS, Azure, or GCP provide a private registry by default when we open an account with them.

- Private Registry


```shell
docker login private-registry.io
```

Once successful, run the application using private registry as part of the image name like this

- Always log in before pulling or pushing images to a private registry

```shell
docker run private-registry.io/apps/internal-app
```

#### Deploy Priavte Registry

`Docker registry` is **itself another application** an it's available as a docker image named as `registry`

```shell
# pull registry image and run the container
docker run -d -p 5000:5000 --name registry registry:2
```
_1st Step: Tag image with private registry URL_

```shell
docker image tag my-image localhost:5000/my-image
```
_2nd Step: Push image_

```shell
docker push localhost:5000/my-image
```
_3rd Step: Pull image_

```shell
docker pull localhost:5000/my-image
docker pull 192.168.56.5000/my-image
```

**Practices**

_Let practice deploying a `registry server` on our own. Run a registry server with name equals to `my-registry` using `registry:2` image with host port set to `5000`, and restart policy set to `always`._

_Note: Registry server is exposed on port `5000` in the image._

```shell
docker run --name my-registry -p 5000:5000 --restart=always registry:2
```

_Now its time to push some images to our registry server. Let's push two images for now .i.e. `nginx:latest` and `httpd:latest`. To check the list of images pushed , use `curl -X GET localhost:5000/v2/_catalog`_

_Note: Don't forget to pull them first._

```shell
docker pull nginx:latest 
docker image tag nginx:latest localhost:5000/nginx:latest 
# and finally push it using 
docker push localhost:5000/nginx:latest
```
```shell
docker pull httpd:latest
docker image tag httpd:latest localhost:5000/httpd:latest
docker push localhost:5000/httpd:latest
```

_Now we can pull images from our `registry-server` as well. Use `docker pull [server-addr/image-name]` to pull the images that we pushed earlier._

```shell
docker pull localhost:5000/nginx
```

_Let's clean up after ourselves. Stop and remove the `my-registry` container_

```shell
docker stop my-registry
docker rm my-registry
```