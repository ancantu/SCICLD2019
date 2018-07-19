# Best Practices for Reproducibility in Research - Reproducible Containers

### Module Objectives

We will be creating reproducible containers on our development systems, and then running the containers at TACC. Once these containers are built once, they can be deployed and run on any system running Singularity in the future irrespective of OS.

Requirements:

- [GitHub account](https://github.com/)
- [Docker Hub account](https://hub.docker.com/)
- Access to Stampede2

Objectives:

- Build a docker container containing specific Software
- Track the `Dockerfile` on GitHub
- Store the container on Docker Hub
- Run the container at TACC

## Tracking your Dockerfile

Docker Hub only tracks end containers, and not the actual recipes used to create them.
Not only does this mean that you don't know how a container was created, but you may not even know what it contains until you run it.

To save yourself headaches in the future, and enable future work, lets begin by creating a GitHub repository to track your Dockerfile.

#### On Github

- Click "New repository"
- Name it tophat (*again*)
- Make it public
- Initialize it with README

Once created, copy the URL to clone it

#### On your local system

Clone your new repository

```
$ git clone https://github.com/username/tophat.git
```

and enter the directory

```
$ cd tophat
```

From here, you can create a `Dockerfile` and then commit it to the repository.

```
$ touch Dockerfile
$ git add Dockerfile
$ git commit -am "Tracking this dockerfile"
$ git push
```

## The Dockerfile

We can build images from a text file called a Dockerfile. You can think of a Dockerfile as a recipe for creating images. The instructions within a dockerfile either add files/folders to the image, add metadata to the image, or both.

### The FROM instruction

All `Dockerfiles` start **FROM** something.
This can either be the bare operating system or a decent base that gives you a convenient starting point, but it should be the first line of your Dockerfile.

When running at TACC, I recommend either starting from

- [gzynda/tacc-base](https://hub.docker.com/r/gzynda/tacc-base/)

or adding the following directories to the OS of your choice

```
RUN mkdir /scratch /work /home1 /gpfs /corral-repl /corral-tacc /data
```

Today, lets start **FROM** gzynda/tacc-base:latest, which is actually Ubuntu 16.04 with some added customizations for running at TACC.

```
FROM gzynda/tacc-base:latest
```

### The RUN instruction

We can run commands *inside* our image by running commands with the **RUN** instruction.
We will use that to install [tophat](https://packages.ubuntu.com/en/xenial/tophat) via apt.
Keep in mind that the the docker build cannot handle interactive prompts, so we use the -y flag in apt.
We also need to be sure to update our apt packages first.

```
FROM gzynda/tacc-base:latest

RUN apt-get update && apt-get install -y tophat
```

Your recipe is now down, and yes, that was fairly straightforward. You just need to build it now.

### Building the container

You can build your container using the docker build command.

```
$ sudo docker build -t [username]/tophat:2.1.1 .
```

You can make sure your container did build by viewing all your local containers with

```
$ sudo docker images
```

### Testing the container

Make sure your container is functional by testing tophat and printing out the help text

```
$ sudo docker run --rm -it [username]/tophat:2.1.1 tophat2 -h
```

## Committing your work

You now have a finished Dockerfile and a functional container.
Lets "save" our work before we accidentally lose track of it.

### Committing your Dockerfile

Assuming you already added your Dockerfile to your github repository, commit your changes and push them to GitHub.

```
$ git commit -am "Finished tophat container"
$ git push
```

You should now see those changes reflected on the webpage.

### Pushing your container

While Docker Hub does not track Dockerfile, it does store images for you or anyone else to pull and run.
To store your image in the cloud, you first need to log into your account on the CLI.

```
$ sudo docker login -u [username]
[enter password]
```

Now that you're logged into your account, you can push the image you just created.

```
$ sudo docker push [username]/tophat:2.1.1
```

After the upload process is complete, you should see your new image online when you log in to https://hub.docker.com

## Running your container @ TACC

Now that your build process and final container are being tracked online, you can pull that image to a TACC system and use it for your work.

First, log in to Stampede2 and start an `idev` session on a compute node for 60 minutes.

```
$ idev -m 60 -p normal
```

Then, load the tacc-singularity module and pull your container from Docker Hub.

```
$ module load tacc-singularity/2.5.1
$ singularity pull docker://[username]/tophat:2.1.1
```

Since all filesystems are mounted in the container because we added those empty mount points, we just need to comment out our module commands in `run_tophat_yeast.sh`.

```
PUBLIC=/work/03076/gzynda/stampede2/ctls-public
VER=Saccharomyces_cerevisiae/Ensembl/EF4
GENES=${PUBLIC}/${VER}/Annotation/Genes/genes.gtf
REF=${PUBLIC}/${VER}/Sequence/Bowtie2Index/genome

SIZE=${1:-500K}
CORES=${2:-8}

# Load standard module
# ml tophat/2.1.1 bowtie/2.3.2


for prefix in WT_{C,N}R_A_${SIZE}; do
        OUT=${prefix}_n${CORES}_tophat
        [ -e $OUT ] && rm -rf $OUT
        tophat2 -p ${CORES} -G $GENES -o $OUT --no-novel-juncs $REF ${PUBLIC}/${prefix}.fastq &> ${OUT}.log
done
```

At this point, we can run it in the container

```
$ singularity exec docker://[username]/tophat:2.1.1 run_tophat_yeast.sh
```

Assuming all went well, you just ran your fully reproducible analysis.

1. Your software environment is reproducible as a Dockerfile
2. Your Dockerfile is under version control on GitHub
3. Your portable container is stored on and made accessible through Docker Hub

## Explore

- Containerize a tool that you use

Back: [Containers](reproducibility_containers_01.md) | Top: [Course Overview](../../index.md)
