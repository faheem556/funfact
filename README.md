# funfact
You can use a `Dockerfile` to build new container images. Dockerfile provides a simple syntax for building Linux or Windows containers. You can learn more about the Dockerfile here (https://docs.docker.com/engine/reference/builder/) Let's build a container for funfact application.

``` 
git pull git@github.com:faheem556/funfact.git
cd funfact

# build dotnet app
docker build -t funfact:dotnet-1-0 -f dotnet/Dockerfile ./funfact/dotnet

# build node image
docker build -t funfact:node-1-0 -f nodejs/Dockerfile ./funfact/nodejs

# build java image
docker build -t funfact:java-1-0 -f java/Dockerfile ./funfact/java
```

Read through the code and docker files to understand the environement. It's really straight-forward.

Let's run the application now.

```
docker run --name funfactd -d --rm -p 8080:80 funfact:dotnet-1-0 
docker run --name funfactn -d --rm -p 8081:80 funfact:node-1-0 
docker run --name funfactj -d --rm -p 8082:80 funfact:java-1-0 
docker ps
```

You should have three running containers at this point. If for some reason the container doesn't start, you can use `docker ps -a` and `docker log` to troubleshoot.

Once the containers are up, open you browsers and try out the applications.

**Cleanup**
```
docker stop funfactd
docker rm funfactd

docker stop funfactn
docker rm funfactd

docker stop funfactj
docker rm funfactj
```
