# Hands-On: Day 4

Let's tie together what we've learned about Docker and Git.

We spent some time already on an Intro to Linux tutorial which we ran on Stampede. Now we will take steps to make sure we can run that same tutorial again in the future in the same environment. But, we need to work together to assemble all the pieces.


1. Select who will be responsible for each part of the tutorial

2. Everyone fork https://github.com/wjallen/ctls2018-class

3. Everyone clone https://github.com/wjallen/ctls2018-class and https://github.com/jamescarson3/ctls2018

4. Find your page of the Linux tutorial in the `ctls2018` repo, and push it to your fork of the `ctls2018-class` repo

5. Put in a pull request to the `wjallen/ctls2018-class` repo

6. Write a Dockerfile that mimics Stampede2 (do this on Jetstream) and mounts the tutorials

7. Run the container interactively on Jetstream, and make sure you can see the tutorial



#### Bonus

What do these series of commands do:

(contents of `Dockerfile`):
```
FROM ubuntu:16.04
RUN apt-get update
RUN apt-get install -y bsdgames
ENV PATH ${PATH}:/usr/games/
```
```
$ sudo docker build -t bsdgames:1.0 ./
$ sudo docker run --rm -it bsdgames:1.0 hangman
```
