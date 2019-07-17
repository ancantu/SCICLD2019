# Cloud Concepts

## What is Cloud? 

- Someone else's computer?
- Programmable, apis, gui, not obviously finite
- Abstracts away the physical computer


## Goals/Benefits

- Resource utilization
- Scalability and elasticity
- Reproducibility by way of programmability
- Reliability through redundancy
- Geographically distributed
- Devops blurs line between operator and developer

## *-as-a-Service

- Clouds can offer different service models:
- Infrastructure-as-a-Service (Iaas) - virtual servers, networks, storage, firewalls, etc. (Openstack, Jetstream, AWS, Azure)
- Platform-as-a-service (PaaS) - deploy applications without managing virtual servers (APIs, TAPIS, Kubernetes)
- Software-as-a-service (SaaS) - Ready to use software (Google Maps, Office365, Github)
- Also: Functions-as-a-Service. (ABACO, AWS Lambda, Google Cloud Functions)

## Comparisons

- Cloud vs HPC
- VMs vs Containers
- Containers vs APIs 

## Cloud Means Different Things to Different Folks

Examples of different kinds of cloud users:

- System Administrators: use cloud to automate operations
  - e.g. We are going to use Ansible to automate
- Software Developers: build applications on cloud servers and platforms
- Computational scientists - Write codes to analysis scientific data in the cloud

## Jetstream 

- The best cloud to use!**
- Infrastructure-as-a-Service (vms, networks, storage volumes, object storage)
- XSEDE Allocated
- https://use.jetstream-cloud.org
- Built using OpenStack

## Openstack

- "OpenStack is a cloud operating system that controls large pools of compute, storage, and networking resources throughout a datacenter, all managed through a dashboard that gives administrators control while empowering their users to provision resources through a web interface."
- https://www.openstack.org
- A huge Open Source collection of projects.
- APIs mostly written in Python.
- Leverages many existing Linux technologies such as virtualization, network bridges, logical volume management, etc.

## Infrastructure-as-a-Service
 
- Virtual Machines (VMs or Instances) - Simulate a physical computer through software.
- crops vs houseplant (cattle vs pets)
- Flavors: (m1.tiny = 1cpu/2gb, m1.small = 2cpu/4gb, m1.medium = 4cpu/8gb, etc.)
- Tools to manage: Ansible, Puppet, Chef, Salt, etc.

## Storage

- Block storage (volumes)
  - Like a USB drive for your virtual machine
  - Can be detached and plugged in to any VM, but only one at a time
  - Like a blank hard drive, must be formatted
  - Standard POSIX filesystem interface (ls, cd, cat, etc.)
- Object storage (s3/swift)
  - Accessed by API (or curl)
  - 2 kinds of objects: buckets and files
  - Every file is a url. e.g. https://www.dropbox.com/s/dh00ija2sk4mswq/hunqapillar.jpg?dl=0
  - May require more effort to make use of in a program
  - Often used for more scalable apps because the file is not necessarily present on the web server
  - Performance implications due to network transfer time

## Network

- Software defined networking: Routers, networks and subnets - used to connect VMs to other computers.
- Private: non-routable ips (not accessible from public internet) (https://en.wikipedia.org/wiki/Private_network)
- Public: routable to public internet
- Note that Docker creates its own private network on top of your vm's private network (which is already virtual in Openstack)

## Security

- Firewall (control access by port, ip)
- Security groups - firewall rules enabling or disabling network traffic to/from ports on the VMs.
- Encryption
- SSH Keys
- SSL/TLS Encryption
- Authentication vs. Authorization
- Multi-Factor Authentication 

## Charge Model Considerations
 
- Cpu
- Memory
- Electricity/Cooling
- Bandwidth
  - Egress/Ingress
- Storage
  - Egress/Ingress

## Containers

- Docker, Singularity (containers)
- Kubernetes, Slurm (orchestration)

## Should we start some VMs?


