## docker-concepts

docker training - building containerized apps with docker

## Docker

## run simple container
```
docker run busybox

docker run -it ubuntu bash

docker ps

docker images

docker version
```
## running container in background
```
docker run jpetazzo/clock

docker run -d imagename

//-d means daemon mode (in background)
```
## logs from the container
```
docker logs containerid

//optional flags

--tail 5 //to get 5 logs from the container log
```
## container history
```
//to see all the container history created

docker ps -a

//optional flags

-a //to view all container history

-q //to view the container id only

-l // to view the last ran container
```
## stopping and killing containers
```
docker stop containerid //to stop the container

docker kill containerid //to kill the container
```
//diiference between kill and stop is that stop is so graceful , thus it send terminate signal to the process and wait for to terminate by its own , wherease kill kills the container immediately by sending kill signal to the container

## creating images in docker
```
//running instannce of a image is called as container

docker search imagename //searches image in docker registery

docker pull imagename:version //instead of starting download the image
```
## building docker images interactively
```
docker run -it ubuntu bash

docker diff containerid

docker commit imageid
```
## tagging name for the images
```
docker commit imageid imagename

docker tag imageid imagename
```
## remove docker container
```
docker rm containerid
```
## remove docker image
```
docker rmi imageid

//optional flags

-f to force the removal of image
```
## Remove all stopped containers
```
docker rm $(docker ps -a -q)
```
## building docker image with dockerfile
```
docker build -t imagename .
```
## docker history and related commands
```
docker history imagename
```
## dockerfile cmd command
```
only one CMD command is executed , if there is more than one CMD command then the last command only executes , previous commands will be overrided

CMD wget -O- -q https://ifconfig.me/ip
```
## dockerfile entry point command
```
ENTRYPOINT ["wget", "-O-", "-q"]
```
## override and enter bash, from entrypoint command in dockerfile with
```
docker run -it --entrypoint bash imagename
```
=============================

## DOCKER CONTAINER NETWORKING BASICS
```
//run a basic webserver as daemon service bg

docker run -d --publish-all jpetazzo/web

// -d shows that container runs in background

// --publish-all makes the container accessible from other container or other computer in network
```
## docker port mapping
```
docker port containerid

docker port containerid portnumber(host)
```
## assign port number to container manually
```
docker run -d -p host:container image
```
## ipaddress of a container
```
docker inspect --format '{{ .NetworkSettings.IPAddress }}' containerid
```
=============================

## Docker for development
```
apache and php on docker

//apacheAndPHPDocker/dockerfile
```
## Mounting volume for development
```
mapping local dev dir -> dir inside a container

docker run -it -p 8888:80 -v hostdir:containerdir ---name contname image name bash

docker run -it -p 8000:80 -v $pwd\www:/var/www/site --name sample server bash

// $pwd provides current working directory

-->then run service apache2 restart
```

## sharing volumes in other container

```
docker run -it --name alpha -v /var/sharedfolder ubuntu bash

//create a text file inside the sharedfolder

echo "this is my shared folder" > myfile.txt

//exit the containers bash

//create another container and make use of previous container shared folder within this container

docker run -it --nama beta --volumes-from alpha ubuntu bash

//now you can read or write the data from shared folder from alpha container

//the main feature of this feature is that , "it can be used as a data container", even the container's shared folder is
//accessible if it is not running interactively, it is the major advantage created by docker



```

## docker inspect

```
//for container
docker inspect alpha

//for image
docker inspect ubuntu

```

## linking containers and talk with each other using ports

```
// for example purpose download redis server

docker pull redis

// run a container using that pulled redis image

docker run -d --name redisserver redis

// now link the above container with the following container

docker run -it --name redisclient1 --link redisserver:redis redis bash

// here redisserver:redis is servername:aliasname

// check the container is linked using the following command

cat /etc/hosts

// we can connect to the linked container using following command

redis-cli -h redis

// now we can set get delete key value pairs in linked container

set myname "yourname"

set mycountry "yourcountry"

//get the key values using following commands

get myname

get mycountry

// we can make link to multiple client servers to the redisserver for example create another client server

docker run -it --name redisclient2 --link redisserver:redis redis bash

// now check the keyvalue pairs that we have created in previously created client

redis-cli -h redis

get myname

get mycountry

// now you should get the same key values that we have created before in client one , 

// thus we can link multiple containers with each other to share resource between them

```

