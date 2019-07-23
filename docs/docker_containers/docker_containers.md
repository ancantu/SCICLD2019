# Introduction to Cloud Computing


This module will be partially interactive. Please follow along on your own computer.


## What is a cloud?

A "cloud" is a computer system that provides users with shared access to on-demand computing resources via the internet.


<center><img src="../../resources/cloud.jpg" style="height:300px;"></center>


### Goals/Benefits
  * Resource utilization
  * Scalability and Elasticity
  * Reproducibility by way of Programmability
  * Reliability through redundancy


### Cloud Means Different Things to Different Groups

Different kinds of people use clouds for different purposes.
  * System Administrators - use cloud to automate operations.
  * Software Developers - build applications on cloud servers and platforms.
  * Computational scientists - Write codes to analysis scientific data in the cloud.

In this course we will try to exposure you to each of these views.

### Cloud service models

Clouds can offer different service models:
  * Infrastructure-as-a-service (Iaas) - virtual servers, networks, firewalls, etc. (Openstack, Jetstream, AWS, Azure)
  * Platform-as-a-service (Paas) - deploy applications without managing virtual servers (Agave, Google App Engine, Heroku)
  * Software-as-a-service (Saas) - Ready to use software application (Jupyterhub, Gmail, Office365)

Also: emerging models such as functioni-as-a-service.

### Infrastrcture-as-a-service

Key concepts:
  * Virtual Machines (VMs) - Simulate a physical computer through software.
  * Software defined networking: Routers, networks and subnets - used to connect VMs to other computers.
  * Security groups - firewall rules enabling or disabling network traffic to/from ports on the VMs.



## Intro to Docker

<!--Intro slides: <https://docs.google.com/presentation/d/1djCYBhKmbDo9L_GZHaarZVUbyrQerPCTctoOAZEVrWk/edit#slide=id.g3c300aa957_0_6> -->

We're going to use an image on Jetstream that already has Docker installed,
but if you'd like to install docker on your laptop here are a few links:

- Docker
  - [Mac](https://docs.docker.com/docker-for-mac/install/)
  - [Windows](https://docs.docker.com/docker-for-windows/install/)
  - [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  - [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
  - [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
  - [Debian](https://docs.docker.com/install/linux/docker-ce/debian/)

### Docker Hub Account
You'll want to create a dockerhub account in order to easily share your containers with other researchers.
To create an account, go to <https://hub.docker.com/>, click `Sign up for Docker Hub`,
and create a username/password to associate with your email.

Typically, accessing the docker daemon requires root or to be in the docker group. For the purposes of this introduction,
 we can simply do everything as the root user:
```
$ sudo su - root
```

Make sure you can access the docker daemon; you can verify this by checking the version:
```
$ docker version
Client:
  Version:           18.09.7
  API version:       1.39
  Go version:        go1.10.1
  Git commit:        2d0083d
  Built:             Wed Jul  3 12:13:59 2019
  OS/Arch:           linux/amd64
  Experimental:      false

Server:
  Engine:
   Version:          18.09.7
   API version:      1.39 (minimum version 1.12)
   Go version:       go1.10.1
   Git commit:       2d0083d
   Built:            Mon Jul  1 19:31:12 2019
   OS/Arch:          linux/amd64
   Experimental:     false

```

Create a test directory to contain your docker work:
```
$ mkdir docker; cd docker
```

We'll also want to login to your Docker Hub account so we can push images:
```
docker login
# Enter username/password
```

### Docker Images and Tags, Docker Hub, and Pulling Images
A Docker image is a container template from which one or more containers can be run. It is a rooted filesystem that,
by definition, contains all of the file dependencies needed for whatever application(s) will be run within the
containers launched from it. The image also contains metadata describing options available to the operator running
containers from the image.

One of the great things about Docker is that a lot of software has already been packaged into Docker images. One source
of 100s of thousands of public images is the official docker hub: <https://hub.docker.com>.

The docker hub contains images contributed by individual users and organizations as well as "official images". Explore
the official docker images here: <https://hub.docker.com/explore/>

Most modern programming languages offer an official image. For example, there is an official image for the Python programming language: <https://hub.docker.com/_/python/>

Docker supports the notion of image tags, similar to tags in a git repository. Tags identify a specific version of an
image.

The full name of an image on the Docker Hub is comprised of components separated by slashes. The components include a
"repository" (which could be owned by an individual or organization), the "name", and the "tag". For example, an image
with the full name
```
jstubbs/test:0.1
```
would be the "test" in the "jstubbs" repository and have a tag of "0.1". TACC maintains multiple repositories on the Docker Hub
including:
```
tacc
taccsciapps
agaveapi
abaco
```
Official images such as the python official image are not owned by a repository, but all other images are.

To pull an image off Docker Hub use the `docker pull` command

```
$ docker pull python
Using default tag: latest
latest: Pulling from library/python
cc1a78bfd46b: Pull complete
. . .
```

As indicated in the output, if no tag is specified the "latest" tag is pulled. You can verify that the image is
available on your local machine using the `docker images` command:
```
$ docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
jstubbs/test           latest              9dfe5a2c4b43        52 minutes ago      81.2 MB
python                 latest              a5b7afcfdcc8        3 hours ago         912 MB
```

### Running a Docker Container
We use the `docker run` command to run containers from an image. We pass a command to run in the container. Similar
to running other programs on Unix systems, we can run containers in the foreground (attached) or in the backgrouns.

#### Running and Attaching to a Container
To run a container and attach to it in one command, use the `-it` flags. Here we run `bash` in a container from the ubuntu image:
```
docker run -it ubuntu bash
root@c10f52ac8ba2:/# ls
root@c10f52ac8ba2:/# exit
ls
```
#### Running a Container in Daemon mode ####
We can also run a container in the background. We do so using the `-d` flag:
```
docker run -d ubuntu sleep infinity
docker ps
docker ps -a
```


### Installing and Running the Containerized Classifier Application

We have packaged up a self-contained image classifier application based on the TensorFlow library. Our image is
available from the DockerHub. To download it, issue the following commnand:

```
$ docker pull taccsciapps/classify_image
```

What you are downloading is a complete application that will run an image classifier algorithm on an image
that is publicly available from the internet.

Once the download is complete, we can run one or more containers from our image. This particular image requires
an environment variable, `$URL`, containing the address to a publicly available image, to be passed in at run time.

For example, we can run our image classifier program on the 3 different images:

<img src="https://raw.githubusercontent.com/TACC/taccster18_Cloud_Tutorial/master/classifier/data/dog.jpeg" height="200">
<img src="https://www.dropbox.com/s/f93aixy0r5f1fz1/jimo.jpeg?raw=1" height="300">
<img src="https://www.dropbox.com/s/80o4vbzrz4eyzp0/mpackard.jpg?raw=1" height="300">

```
$ docker run -it --rm -e URL=https://raw.githubusercontent.com/TACC/taccster18_Cloud_Tutorial/master/classifier/data/dog.jpeg taccsciapps/classify_image
$ docker run -it --rm -e URL=https://www.dropbox.com/s/f93aixy0r5f1fz1/jimo.jpeg?raw=1 taccsciapps/classify_image
$ docker run -it --rm -e URL=https://www.dropbox.com/s/80o4vbzrz4eyzp0/mpackard.jpg?raw=1 taccsciapps/classify_image
```


### Additional Remarks on Running Containers

#### Running Additional Commands in a Running Container ####
Finally, we can execute commands in a running container using the `docker exec` command. First, we need to know the container id, which we can get through the `docker ps` command:

```
~ $ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
e372ff3fde69        ubuntu              "sleep infinity"    2 minutes ago       Up 2 minutes                                   jovial_lewin
```

Here we see the container id is `e372ff3fde69`. To execute `bash` in this container we do:
```
docker exec -it e372ff3fde69 bash
ls
exit
```
At this point we are attached to the running container. If our bash session exits, the container will keep running because the `sleep infinity` command is still running.

*Note: The `docker ps` command only shows you running containers - it does not show you containers that have exited. In order to see all containers
on the system use `docker ps -a`.

```
root@js-104-88:~/docker# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
e372ff3fde69        ubuntu              "sleep infinity"    2 minutes ago       Up 2 minutes                                   jovial_lewin
c10f52ac8ba2        ubuntu              "bash"              7 minutes ago       Exited (0) 6 minutes ago                       focused_payne
```


### Removing Docker Containers ###
You can stop the container with the `docker stop` command. Pass either the container ID or the Name to docker stop:
```
docker stop e372ff3fde69
docker stop jovial_lewin
```
We can remove a docker container using the `docker rm` command (optionally passing `-f` to "force" the removal if the container is running). You will need the container name or id to remove the container:
```
$ docker rm -f e372ff3fde69
```


These IDs will be unique for your container.

### Building Images From a DockerFile
We can build images from a text file called a Dockerfile. You can think of a Dockerfile as a recipe for creating images.
The instructions within a dockerfile either add files/folders to the image, add metadata to the image, or both.

#### The FROM instruction
We can use the `FROM` instruction to start our new image from a known image. This should be the first line of our Dockerfile. We will start our image from an official Ubuntu 16.04 image:

```
FROM ubuntu:latest

```

#### The RUN instruction
We can add files to our image by running commands with the `RUN` instruction. We will use that to install `wget` via `apt`. Keep in mind that the the docker build cannot handle interactive prompts, so we use the `-y` flag in `apt`. We also need to be sure to update our apt packages.

The Dockerfile will look like this now:
```
FROM ubuntu:16.04

RUN apt-get update && apt-get install -y wget
```

#### The ADD instruction
We can also add local files to our image using the `ADD` instruction. We can add a file `test.txt` in our local directory to the `/root` directory in our container with the following instruction:

```
ADD test.txt /root/text.txt
```

A complete Dockerfile for the classify_image application as well as the necessary scripts and supporting files is available from the Tutorial gihub repo:
<https://github.com/TACC/taccster18_Cloud_Tutorial/tree/master/classifier>


#### Building the Image
To build an image from a Dockerfile we use the `docker build` command. We use the `-t` flag to tag the image: that is, give our image a name. We also need to specify the working directory for the buid. We specify the current working directory using a dot (.) character:
```
docker build -t classify_image .
```

Note that this command requires all the files in the `classifier` directory in the Tutorial github repo (https://github.com/TACC/taccster18_Cloud_Tutorial/tree/master/classifier)

Next: [Docker Compose](../docker_compose/docker_compose.md) | Top: [Course Overview](../../index.md)
