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

## docker-hub upload and download images

```
// to login and logout

docker login 

docker logout

// before we upload our custom image to docker-hub we need to tag our custom image with id using following command

docker tag imagename dockerid/imagename

// in my case i am going to tag a sample apache image that we have created during volume learning lecture

docker tag sampleapache thayalangr/apachesample

// now you can see your tagged image under docker images

docker images

// now push your tagged custom image to docker-hub using following command

docker push tagged-imagename

// in my case 

docker push thayalangr/apachesample

// after successfull push now you can able to see your custom docker image in your docker registery

// visit https://hub.docker.com to see your uploaded docker image

// yeah!!! we have uploaded our custom image to docker registery

// now we can able to download or ship our docker image 

// we can pull it directly from docker-hub using following command

docker pull taggedimagename

// in my case

docker pull thayalangr/apachesample

// now you can able to run in any docker machine by pulling like this manner

// we can (push or upload or provide update) to our custom image using following command

docker push thayalangr/apachesample:tagname

// so we can serve multiple versions our image to docker registery

// for pulling the specific version or tag of our custom image 

// we should specify the tag name along with it

docker pull thayalangr/apachesample:tagname

```
## glossary

### thus we have learned most frequently used concepts of docker such as

#### run-simple-container
 [run-simple-container](#run-simple-container)
#### running-container-in-background
 [running-container-in-background](#running-container-in-background)
#### logs-from-the-container
 [logs-from-the-container](#logs-from-the-container)
#### container-history
 [container-history](#container-history)
#### stopping-and-killing-containers
 [stopping-and-killing-containers](#stopping-and-killing-containers)
#### creating-images-in-docker
 [creating-images-in-docker](#creating-images-in-docker)
#### building-docker-images-interactively
 [building-docker-images-interactively](#building-docker-images-interactively)
#### tagging-name-for-the-images
 [tagging-name-for-the-images](#tagging-name-for-the-images)
#### remove-docker-container
 [remove-docker-container](#remove-docker-container)
#### remove-docker-image
 [remove-docker-image](#remove-docker-image)
#### remove-all-stopped-containers
 [remove-all-stopped-containers](#remove-all-stopped-containers)
#### building-docker-image-with-dockerfile
 [building-docker-image-with-dockerfile](#building-docker-image-with-dockerfile)
#### docker-history-and-related-commands
 [docker-history-and-related-commands](#docker-history-and-related-commands)
#### dockerfile-cmd-command
 [dockerfile-cmd-command](#dockerfile-cmd-command)
#### dockerfile-entry-point-command
 [dockerfile-entry-point-command](#dockerfile-entry-point-command)
#### override-and-enter-bash-from-entrypoint-command-in-dockerfile-with
 [override-and-enter-bash-from-entrypoint-command-in-dockerfile-with](#override-and-enter-bash-from-entrypoint-command-in-dockerfile-with)
#### docker-container-networking-basics
 [docker-container-networking-basics](#docker-container-networking-basics)
#### docker-port-mapping
 [docker-port-mapping](#docker-port-mapping)
#### assign-port-number-to-container-manually
 [assign-port-number-to-container-manually](#assign-port-number-to-container-manually)
#### ipaddress-of-a-container
 [ipaddress-of-a-container](#ipaddress-of-a-container)
#### docker-for-development
 [docker-for-development](#docker-for-development)
#### mounting-volume-for-development
 [mounting-volume-for-development](#mounting-volume-for-development)
#### sharing-volumes-in-other-container
 [sharing-volumes-in-other-container](#sharing-volumes-in-other-container)
#### docker-inspect
 [docker-inspect](#docker-inspect)
#### linking-containers-and-talk-with-each-other-using-ports
 [linking-containers-and-talk-with-each-other-using-ports](#linking-containers-and-talk-with-each-other-using-ports)
#### docker-hub-upload-and-download-images
 [docker-hub-upload-and-download-images](#docker-hub-upload-and-download-images)
