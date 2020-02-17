# Docker Setup Guideline
This is a step-by-step guide to install Docker.

## Install Docker

```sh
## Update the apt package index:
sudo apt-get update

## Install necessary packages:
sudo apt-get install  -y systemd apt-transport-https ca-certificates curl gnupg-agent software-properties-common

## Add Docker's official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

## Add the apt repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

## Update the apt package index:
sudo apt-get update

## Install docker-ce:
sudo apt-get -y install docker-ce docker-ce-cli containerd.io

## Add your user to the docker group with the following command.
sudo usermod -aG docker $(whoami)

## Set Docker to start automatically at boot time:
sudo systemctl enable docker.service

## Finally, start the Docker service:
sudo systemctl start docker.service

## Then we'll verify docker is installed successfully by checking the version:
docker version
```

## Install Docker Compose
```sh
## Install python-pip
sudo apt-get install -y python-pip

## Then install Docker Compose:
sudo pip install docker-compose

## To verify the successful Docker Compose installation, run:
docker-compose version
```
## Basic Usage

### Run

```sh
## Run and explore container file
docker run -t -i --net=host $IMAGE
```

### Build
```sh
docker build . -t $TAG -f /path/to/Dockerfile
docker run -d $TAG
```

### Containers
```sh
#Create a container from an image:
docker container create $IMAGE_PATH

#Start an existing container:
docker container start $CONTAINER_ID

#Create a new container and start it:
docker container run $CONTAINER_ID

#List running containers:
docker container ls

#List all containers (include non-running ones):
docker container ls -a

#Stop all running containers:
docker stop $(docker ps -aq)

#Restart all containers:
docker restart $(docker ps -a -q)

#Remove all containers:
docker rm $(docker ps -aq)

#Inspect a container:
docker container inspect $CONTAINER_ID

#Print a container’s logs:
docker container logs $CONTAINER_ID

#Gracefully stop running container:
docker container stop $CONTAINER_ID

#Stop main process in container abruptly:
docker container kill $CONTAINER_ID

#Delete a stopped container:
docker container rm $CONTAINER_ID

#Delete all not-running containers:
docker container rm $(docker ps -a -q)
```

### Images
```sh
#Build an image:
docker image build $IMAGE_PATH

#Push an image to a remote registry:
docker image push $IMAGE_PATH

#List images:
docker image ls

#See intermediate image info:
docker image history $IMAGE_ID

#See lots of info about an image, including the layers:
docker image inspect $IMAGE_ID

#Delete an image:
docker image rm $IMAGE_ID

#Delete all images:
docker image rm $(docker images -a -q)
```
### Misc
```sh
#List info about your Docker Client and Server versions:
docker version

#Log in to a Docker registry:
docker login

#Delete all unused containers, unused networks, and dangling images:
docker system prune
```


## Troubleshooting

#### Setting locale failed:
```
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
```
Resolution:
```sh
export LANG=en_US.UTF-8
export LANGUAGE=$LANG
export LC_ALL=$LANG
```
#### Unsupported locale setting:
```
locale.Error: unsupported locale setting
```
Resolution:
```sh
export LC_ALL=C
```