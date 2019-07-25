# TACC as Cloud

One of the major efforts here at TACC is to make the significant advanced computing resources more easily and more widely used.  
One way to do this is to help ensure that the software run uses the entire node and uses it efficiently.  
If each science job is accomplished faster, that allows more science to be performed.  
A system like Stampede2, the largest system currently accessible by any scientist, has 10x demand for its resources than is available.  
To address this, TACC staff will install commonly used scientific software as a `module`.  
These `module` installations are often optimized for the system.

The use of `module` also allows TACC users to more quickly get started.  
If there software is already installed, then they do not have to go through the installation themselves.
To allow TACC users to easily leverage the vast array of software containers, on most of our major systems, we have `singularity` available as a `module`.

```module load tacc-singularity```

## Beyond the Command line

For many scientists, the command line interface can be barrier for computation.  
As science becomes increasingly collaborative, it can be beneficial to reduce these barriers to sharing computing steps and results.
For this reason, TACC is developing Science-themed Gateways or Portals that allow users to leverage the computing resources at TACC without accessing the command line.

[Science Gateways at TACC](https://raw.githubusercontent.com/ancantu/SCICLD2019/master/docs/tacc_as_cloud/SciCloud2019_TACCasCloudSlides.pdf)

## Using a Portal

We'll interactively explore the UTRC portal using TACC training accounts.

## Building a Private Portal App

Software is made available on TACC Portals as an `App`.  
Whether you have installed software that isn't yet an App, or if you are developing your own software, you may want to share your installation with others on a TACC Portal.
The increases reproducibility and accessibility for users of this software.
You can install a `Private App` in a TACC Portal which only you can access initially.  
Later, you can share this App with others on your research team.  Eventually, you can request the App be made public to anyone using the Portal.

### Step 1 - Setup Private Systems on Portal

There are command-line ways to do this, but we accomplished this step already while exploring the UTRC portal.
By running a public app, we created a private system linked to our training allocation.


### Step 2 - Test your software and write scripts

Before going through the process of registering an app with the portal interface, 
you'll want to make sure the software works as intended.

Login to Stampede2

```ssh username@stampede2.tacc.utexas.edu```

Change into a work directory for developing apps and clone sample app

```
cdw
mkdir apps
cd apps
git clone https://github.com/jamescarson3/wc-app
```

Submit the test script to run in the queue

```
sbatch test.sh
showq -u
``` 

After it runs, check your output

```
ls
less wc.out
```

You can also example the error and out files.


### Step 3 - Connect to Portal tenant

The UTRC portal is still in development and the rest of the process for registering private apps is evolving rapidly.  
In the coming months, you should be able do these steps using the portal itself.
Right now, however, we will use command line tools.  
We have containerized these tools to simplify installation.  
If you were on your laptop or Jetstream, you could just run docker

```docker run --rm -it jurrutia/tapis_cli:3.0.3```

However, since we are working on Stampede2, we need singularity, which requires a compute node.
We will request an interactive compute node with the idev command

```
idev
```

Once we are on the node, we will load the singularity module and launch the interactive container.

```
ml tacc-singularity
singularity shell docker://jurrutia/tapis_cli:3.0.3
```

Inside the container, connect to `portals` tenant using

```auth-session-init```

Enter training account username, and then enter password twice

Once connected, we will use the following CLI commands to verify that we connected correctly.

```
apps-list
systems-list
files-list agave://utrc-home.jcarson
```

For the files-list command above, you will need to change it to match your own username

### Step 4 - Register your private App 

You need to update the app json file.
Specifically, the "owner", "executionSystem", "deploymentPath" and  "deploymentSystem"

Once the json file is ready, you can register it as a private app

```
apps-addupdate -F wc-0.0.1.json
```

If you receive a success message, you can now try running the app in the portal.


### On your own:

Try creating your own private fastqc app using singularity.  
You can use these files as a starting point:  https://github.com/wonaya/utrc_fastqc


