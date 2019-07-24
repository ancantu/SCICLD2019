## Build an App with a Github-Docker Integration

Imagine this near-real-world example:
You have some code or an application that you are developing in python.
You want to (1) easily run this code on your VMs distributed around different platforms,
and (2) make the latest version of the code available for others to use without worrying about complicated installations or dependencies.

Here is an example of how you might achieve this: You work on the python code on your laptop or lab computer.
You commit code periodically to a Github repository.
Commits to the repository automatically trigger a re-build of your docker image on Docker Hub.
Ideally, you would also incorporate unit tests, continuous integration, etc., but that is beyond the scope of this example.

<br>

#### What do you need

Dockerhub account, Github account, local development environment with docker and git installed. (A Jetstream VM is fine for the "local" development environment).

<br>

#### Your python script

You have written a python script that estimates the value of pi with great efficiency! Create a folder for your project called `python-pi`:

```
$ mkdir python-pi
$ cd python-pi
```

And copy and paste the following code into a file called `pi.py`:
```
$ vim pi.py
```
```
#!/usr/bin/env python3
from random import random as r

def main():
    num_inside = 0
    num_tries = 100000

    for i in range(num_tries):
        if (((r()**2)+(r()**2)) < 1):
            num_inside += 1

    print ('the pi estimate after %d points is %f' % (num_tries, 4.0*num_inside/num_tries))

if __name__ == '__main__':
    main()
```

<br>

#### Containerize it

Now let's make a simple container in which your code will run. It has very few dependencies, so it should be easy. Alpine Linux is a Linux distribution designed to be very small - only around 5MB! This makes it ideal for simple, small containers. Create a `Dockerfile` with the following contents:
```
$ vim Dockerfile
```
```
FROM frolvlad/alpine-python3

ADD pi.py /usr/bin/pi
RUN chmod +x /usr/bin/pi
```

<br>

#### Test your code locally

Build the container with the following command:
```
$ docker build -t python-pi .
```

Test the code with the following command:
```
$ docker run --rm -it python-pi pi
the pi estimate after 100000 points is 3.134880
```

Not a bad estimate of pi for 100,000 random numbers!

<br>

#### Push the assets to Github

Navigate to github.com/YOUR-GITHUB-USERNAME and click on Repositories => New.

Give it the name `python-pi`, just like the name of the folder, and click Create.

Finally, in your local folder, issue the following commands:

```
git init
git add *
git commit -m "first commit"
git remote add origin https://github.com/YOUR-USERNAME-HERE/python-pi.git
git push -u origin master
```

<br>

#### Push the image to Docker Hub

Make the image in its current form available to the world:
```
$ docker tag python-pi USERNAME/python-pi   # you may need to rename
$ docker push USERNAME/python-pi
```
(Don't forget to replace USERNAME with your Docker Hub username).

<br>


#### Set up the Github-Docker Hub integration

Navigate to your new Docker image in a web browser, which should be at an address similar to:

https://cloud.docker.com/repository/docker/YOUR-DOCKER-USERNAME/python-pi

Click on Builds => Link to Github. (If this is your first time connecting a Docker repo to a Github repo, you will need to set it up. Press the 'Connect' link to the right of 'Github'. If you are already signed in to both Docker and Github in the same browser, it takes about 15 seconds to set up).

Once you reach the Build Configurations screen, you will select your Github username and repository named `python-pi`.

Leaving all the defaults selected will cause this Docker image to rebuild every time you push code to the master branch of your Github repo. For this example, keep the defaults.

Click 'Save and Build' and cross your fingers.

<br>


#### Bringing it home

The final step is to reconsider what our goal was. We wanted to develop code and immediately make it available for our VMs and for others to use without worrying about installation and dependencies.

Running our local container does the following:
```
$ docker run --rm -it python-pi pi
the pi estimate after 100000 points is 3.135760
```

Edit the code to use 100,000,000 points instead of 100,000 points:
```
$ vim pi.py
```
```
#!/usr/bin/env python3
from random import random as r

def main():
    num_inside = 0
    num_tries = 100000000     # changed this line

    for i in range(num_tries):
        if (((r()**2)+(r()**2)) < 1):
            num_inside += 1

    print ('the pi estimate after %d points is %f' % (num_tries, 4.0*num_inside/num_tries))

if __name__ == '__main__':
    main()
```

Push the modified python code to Github:
```
$ git add pi.py
$ git commit -m "updating num_tries to try more points"
$ git push
```

Now check the online Github repo to make sure your change is there, and check the Docker hub repo to see if your image is automatically rebuilding. Once it is done rebuilding, remove your local copy of the image, pull the fresh image, and test if the code is updated inside:
```
$ docker rmi USERNAME/python-pi:latest
$ docker pull USERNAME/python-pi:latest
$ docker run --rm -it USERNAME/python-pi:latest pi
the pi estimate after 100000000 points is 3.141400
```
(Don't forget to replace 'USERNAME' with your Docker hub username).

If the pi estimate is based on the modified number of points, it is a success!

<br>

Top: [Course Overview](../reproducibility.md) |
