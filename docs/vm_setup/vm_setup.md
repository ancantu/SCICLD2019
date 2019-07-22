# Initial setup

First we need to login to Atmosphere:
1. To start the VM provisioning process, navigate to <https://use.jetstream-cloud.org>.

2. Click Login in the top right to authenticate using your XSEDE credentials:
<img src="../../resources/login.webp" align="center">
3. On the Globus Auth screen select XSEDE and click Continue.
<img src="../../resources/globus.webp" align="center">

4. Enter your XSEDE credentials. Best results will occur if you treat your username as if it were case-sensitive when using Jetstream. <br>
<img src="../../resources/xsedecred.webp">


5. After you type in your XSEDE username and password, confirm whether you will allow your credentials to be used to access Jetstream.  You may wish to review the terms of service and privacy policies linked on that page. Generally, this screen will only be displayed the first time you log into Jetstream, however, changes to Globus Auth might cause this screen to be displayed on a later login to Jetstream.  To use Jetstream, click Allow.
<img src="../../resources/webapp.webp">


6. To proceed, click  Allow and the web interface to Jetstream will load.
<img src="../../resources/atmo-loading.png">

7. Once authenticated via Globus Auth, the Jetstream Dashboard will be displayed.  On this page you will be able to:

     * launch a new instance
     * browse help resources
     * change settings
     * view resources and usage history
     * view a Jetstream Community Activity feed
<img src="../../resources/atmo-dashboard.png">


### Launch an Instance

1. Jetstream virtual machines (VMs) are organized by projects. To create a new project, click on <img src="../../resources/atmo-projects-button.png" height=30> from any screen. To create a new project click <img src="../../resources/atmo-create-proj-button.png" height=30>. Fill out the name and description. <br><img src="../../resources/atmo-create-project.png" width="40%">.

2. To get started using a Jetstream virtual machine, click Launch New Instance from the Dashboard screen. On the list of images page, scroll through the the list of images or enter an image name, tag or description in the search box. For instance, to locate images named or tagged with “Docker”, enter that text in the search bar. The search is not case sensitive.  
  <img src="../../resources/atmo-image-select.png" width="50%">


3.  On the Launch an Instance / Basic Options screen:

    * Enter a name for the instance
    * Select the image version if there are multiple versions available
    * Select or create a project to hold this instance. If you have any existing projects, they will be shown here and you can select one. If you don't have any existing projects, click Create New Project and fill in Project Name and Description. A detailed description is optional, but it is recommended to include any grant names or other easily identifying details so others working with you may easily find it. Click Create to create the new project.
    * Indicate the allocation source
    * Choose the provider to run on, Indiana or TACC
    * Choose the instance size. This indicates the vCPUs, memory, and disk size for the VM. See the Virtual Machine Sizes table to show the available options and the SUs consumed per hour.  Check projected resource usage: Allocation Used and Resources Instance will Use.
    * Click Launch Instance to start the initialization and build of the instance.
    <img src="../../resources/atmo-instance-launch.png" width="80%">

4. When the instance is finished building and deploying, you'll see the label changes to *Status* "<font style="color:Green;">● </font> **Active**" and shows the IP Address: <br> <img src="https://iujetstream.atlassian.net/wiki/download/thumbnails/17465484/active.jpg?version=1&modificationDate=1457464079048&cacheVersion=1&api=v2&width=808&height=188">

5. Click on the Instance name to see the characteristics of the Instance as well a various [management actions](https://wiki.jetstream-cloud.org/Instance+management+actions) that can be taken.
 + Image: create a new Image from this Instance)
 + Suspend: put the Instance to "sleep"
 + Shelve: shutdown and store the Instance
 + Stop: shutdown the Instance but do not store
 + Reboot: restart the instance
 + Redeploy: renew the cloud and IP status of the instance
 + **DELETE**: remove the Instances
 + Open Web Shell: Open a command line window to the Instances
 + Open Web Desktop: Open a VNC Desktop to the Instances

### Using WebShell to Access Your Instance
WebShell (based on Guacamole) provide a web-based alternative to terminal command line software for rapid access or for those times when you installing a terminal is not an option.
 + Jump to [web shell documentation](https://wiki.jetstream-cloud.org/Logging+in+with+Web+Shell+-+also+copying+and+pasting)

### Using Terminal Software to SSH Into Your Instance
First you'll want to find your IP Address:

<img src="../../resources/IP_address.png" width="800" height="380">

### MacOS X & Unix/Linux
For Mac OS X open a terminal window (from Finder, go to Applications, click Utilities, and then double-click Terminal).
For Linux, there are many terminal options, including xterm, konsole, or gnome-terminal.
In the terminal window, enter the following command, using your XSEDE username and the instance IP address:
```
ssh <your_xsede_username>@<instance_ip_address>
```

Press Enter.
A successful login will look similar to the following:

<img src="../../resources/ssh.jpg">

### Windows using PuTTY
PuTTY is an SSH client for Windows.  It operates a bit differently than Terminal to make the initial SSH connection. For a useful guide to using PuTTY, see [PuTTY – Remote Terminal and SSH Connectivity](https://support.suso.com/supki/SSH_Tutorial_for_Windows).

1. Download the PuTTY application.
2. Launch PuTTY.
3. The first time PuTTY is used for login, add your private key.
  * Single click the "Default Settings" session to save your private key for all future sessions.
  * Click on the + symbol next to the 'SSH' category on the left hand side.
  * Click on the 'Auth' category to bring up the PuTTY Configuration screen (see screenshot below).
  * The key is set down at the bottom under 'Private key file for authentication'. Click on the Browse button next to the 'Private key file for authentication' field and locate your private key file on the file system. Select the file and press 'Ok'. (It is probably in your My Documents folder. )
  * Click the 'Session' category from the left hand side.
  * Make sure "Default Settings" is still selected.
  * Click Save.

Enter the IP address, either copied from your My Instances list or from the confirmation email, and click Connect.
Enter your XSEDE username when prompted for a login name and click Enter.

<img src="../../resources/Putty-config-sshauth.png">

If you decide to use the web shell instead of ssh-ing in you'll likely want to setup the webshell to accept copy/paste from your clipboard:
<https://iujetstream.atlassian.net/wiki/spaces/JWT/pages/141525076/Logging+in+with+Web+Shell+-+also+copying+and+pasting>


Next: [Building Containers](../docker_containers/docker_containers.md) | Top: [Course Overview](../../index.md)
