# Docker Workshop
Lab 07: Building an asp.net Windows container

---

## Taks

 - Your task is to build Asp.net container as a windows Container :
  
   1. Goto Demos/windows-container and inspect the code ( Using VS is not required)

   2. Change your Docker build target from Linux container to Windows Containers

   3. Build the docker and run it
    
```
$ docker build -t <your_image> .
$ docker run --name aspnet_sample --rm -it -p 8000:80 <your_image>
```

   4. How much does a window image space is used on Disk ? 


## Reference
https://github.com/microsoft/dotnet-framework-docker/tree/master/samples/aspnetapp