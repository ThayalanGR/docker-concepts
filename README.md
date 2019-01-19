# docker-concepts

docker training - building containerized apps with docker

# Docker

# run simple container

docker run busybox
docker run -it ubuntu bash
docker ps
docker images
docker version

# running container in background

docker run jpetazzo/clock
docker run -d imagename
//-d means daemon mode (in background)

# logs from the container

docker logs containerid
//optional flags
--tail 5 //to get 5 logs from the container log

# container history

//to see all the container history created
docker ps -a
//optional flags
-a //to view all container history
-q //to view the container id only
-l // to view the last ran container

# stopping and killing containers

docker stop containerid //to stop the container
docker kill containerid //to kill the container
//diiference between kill and stop is that stop is so graceful , thus it send terminate signal to the process and wait for to terminate by its own , wherease kill kills the container immediately by sending kill signal to the container

# creating images in docker

//running instannce of a image is called as container
docker search imagename //searches image in docker registery
docker pull imagename:version //instead of starting download the image

# building docker images interactively

docker run -it ubuntu bash
docker diff containerid
docker commit imageid

# tagging name for the images

docker commit imageid imagename
docker tag imageid imagename

# remove docker container

docker rm containerid

# remove docker image

docker rmi imageid
//optional flags
-f to force the removal of image

# Remove all stopped containers

docker rm \$(docker ps -a -q)

# building docker image with dockerfile

docker build -t imagename .

# docker history and related commands

docker history imagename

# dockerfile cmd command

only one CMD command is executed , if there is more than one CMD command then the last command only executes , previous commands will be overrided

CMD wget -O- -q https://ifconfig.me/ip

# dockerfile entry point command

ENTRYPOINT ["wget", "-O-", "-q"]

# override and enter bash, from entrypoint command in dockerfile with

docker run -it --entrypoint bash imagename

=============================

# DOCKER CONTAINER NETWORKING BASICS

//run a basic webserver as daemon service bg
docker run -d --publish-all jpetazzo/web
// -d shows that container runs in background
// --publish-all makes the container accessible from other container or other computer in network

# docker port mapping

docker port containerid
docker port containerid portnumber(host)

# assign port number to container manually

docker run -d -p host:container image

# ipaddress of a container

docker inspect --format '{{ .NetworkSettings.IPAddress }}' containerid

=============================

# Docker for development

apache and php on docker
//apacheAndPHPDocker/dockerfile
