# Docker Workshop
Lab 06: Building more complex images

---

## Taks

 - Your task is to fix the Asp.net core app in src/06-core-webapp to have the following characteristics :
  
   1. change the hard coded port number from 3001 to Environment variable ( Program.cs line 24)

   2. Add your name as the Author in the Docker file

   3. Complete the TODO parts in the Dockerfile.
    use https://github.com/dotnet/dotnet-docker/blob/master/samples/dotnetapp/Dockerfile as Reference.

   4. Add the Port number to the docker file so it will be used as environment variable 

   5. Change the docker file so that it will not allow to override it with a different command.


## Solution 
 - src/06-core-webapp-final
