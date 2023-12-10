How to create your own image

It clould either be because you can't find  a component or a service that you want to use as part of your application on Docker Hub or you and your team decided that the application you are developing will be dockerized for ease of shipping and deployment.

How to create my own image ?

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
FROM Ubuntu

RUN apt update
RUN apt install python

RUN apt install python-pip
RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

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