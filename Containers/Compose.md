
### Docker Compose

If we need to set up a complex application running multiple services, a better way to do it is to use `Docker Compose`. With the compose file we cloud create a configuration file in `.yml` format called `Docker-compose.yml`.    


#### Sample application - voting application

docker run links

Link is a command line option, which can be used to link two containers together

```shell
# docker run -d --name=vote -p 5000:80 --link <container_name>:<host_app_name> voting-app
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app
```

এখন যদি আমি voting-app container এর ঢুকে `cat /etc/hosts` command run করি তবে IP সহ redis এর একটা entry দেখতে পাব 

`Docker-compose` by default **bridged network** use করে 