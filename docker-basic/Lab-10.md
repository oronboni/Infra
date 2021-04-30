# Docker Workshop
Lab 10: Working with Docker Hub (Registry)

---

## Preparations

 - Clean your docker host using the commands (in bash):

```
$ docker rm -f $(docker ps -a -q)
```


## Instructions

 - Browse to docker hub and sign up:
```
https://hub.docker.com
```

 - Create a new folder:
```
$ mkdir my-app
$ cd my-app
```

 - Create a index.html file with the content below:
```
<!DOCTYPE html>
<html>
<body>
<h2>Radio Buttons</h2>
<form>
  <input type="radio" name="language" value="C#" checked>David<br>
  <input type="radio" name="language" value="Python">Moises<br>
  <input type="radio" name="language" value="NodeJs">Daniel<br>  
</form> 
<button>Vote!</button> 
</body>
</html>
```

 - Create a Dockerfile with the content below:
```
FROM ngnix:alpine
COPY . /usr/share/ngnix/html
```

 - Build the docker image::
```
$ docker build -t voting-app:latest .
```

 - Run the application to ensure that everything it's ok:
```
$  docker run -p 80:80 voting-app:latest
```

 - Browse to the application page:
```
http://localhost:80
```

 - Let's create two tags for the image we want to push to docker hub:
```
$ docker tag voting-app:latest <your-docker-hub-username>/voting-app:1.0
$ docker tag voting-app:latest <your-docker-hub-username>/voting-app:latest
```

 - Login to docker hub from the terminal:
```
$ docker login
Login with your Docker username to push and pull images from Docker Hub. 

Username (): <your-docker-hub-username>
Password: <your-docker-hub-password>
```

 - Push the images to docker hub:
```
$ docker push <your-docker-hub-username>/voting-app:1.0
$ docker push <your-docker-hub-username>/voting-app:latest
```

 - See the pushed images in the docker hub page:
```
https://hub.docker.com/r/your-docker-hub-username/voting-app/tags/
```

- **Challenge** : Consume the image from Docker hub by running an instance of your app