# Docker Workshop
Lab 05: Building your first "Hello World" Dotnet core container

---

## Preparations

 - Clean your docker host using the commands (in bash):

```
$ docker rm -f $(docker ps -a -q)
```

## Task
1. In this task you are asked to build and run a simple .NetCore "hello-world" application
2. You can start fresh and create the project (only .NetCore ) or take the files from src/05-hello-core
3. After the container runs , replace the command that the container runs , override it with "echo override!".


Remark : if you prefer to write it using Python/Java or NodeJs the starter projects are under src/05-hello-java/node/python

## Instructions

 - Browse to the course root folder ,and redirect to the src/05-hello-core using you favorite Terminal.
```
$ cd C:/Source/docker-workshop/src/05-hello-core
```

 - Inspect the project build/publish using the .net core cli tools.
```
$ dotnet publish -c "Release" --output ./bin/Release
```
(In a later tutorial we shall see how to build the docker image in the Dockerfile)

```
FROM mcr.microsoft.com/dotnet/runtime:3.1

COPY bin/Release/ /app

WORKDIR /app
CMD ["dotnet", "hello-core.dll"]

```
 - Browse to https://hub.docker.com/_/microsoft-dotnet-runtime/ and find the to which actual OS layer this runtime belongs.
 
 - Your manager tells you the image got vulnerable , and you must use Ubuntu:20 to run .net core. what is the new image tag  ?
 - Build the image using:
```
$ docker build -t hello-core:1.0 .
```

 - Ensure the image was created:
```
$ docker images
```
 - What is the image size on Disk ?
 - Run the created image and verify its output:
```
$ docker run hello-core:1.0
```
 - What happens to the container after the command is finished ? 
 
 - Now, run the same image but passing a command as run parameter (stop and remove the previous running instance):
```
$ docker run hello-core:1.0 ls -al
```

 - As you may have noticed the command inside the container was overridden by the command passed as parameter in the docker run command

 - How many containers are on your system now ? at what phase ? 
