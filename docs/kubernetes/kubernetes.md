## Kubernetes

Kubernetes is an orchestration tool that allows management of conainers across multiple nodes.

## Overview

* Pod: Smallest deployable unit. Consists of 1 or more containers. Kind of like "localhost".
* Deployment: Multiple pods.
* Service: Expose a pod or deployment to network.
* Volume: Attach storage.
* ConfigMap: Store strings or files for pods to use.
* Secret: Cncrypted configmap.

...many more

## Kube install

Once ansible has finished ( [Ansible](ansible.md) ), you can check that everything is up:

    kubectl get all --all-namespaces

Should look something like this (node everything in "Running" status):
    
    NAMESPACE     NAME                                                        READY   STATUS    RESTARTS   AGE
    kube-system   pod/coredns-5c98db65d4-v6kr6                                1/1     Running   0          25m
    kube-system   pod/coredns-5c98db65d4-v8bwz                                1/1     Running   0          25m
    kube-system   pod/etcd-js-17-151.jetstream-cloud.org                      1/1     Running   0          24m
    kube-system   pod/kube-apiserver-js-17-151.jetstream-cloud.org            1/1     Running   0          24m
    kube-system   pod/kube-controller-manager-js-17-151.jetstream-cloud.org   1/1     Running   0          24m
    kube-system   pod/kube-flannel-ds-amd64-dsg87                             1/1     Running   0          6m1s
    kube-system   pod/kube-proxy-gf4st                                        1/1     Running   0          25m
    kube-system   pod/kube-scheduler-js-17-151.jetstream-cloud.org            1/1     Running   0          24m
    
    NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
    default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  25m
    kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   25m
    
    NAMESPACE     NAME                                     DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                     AGE
    kube-system   daemonset.apps/kube-flannel-ds-amd64     1         1         1       1            1           beta.kubernetes.io/arch=amd64     6m1s
    kube-system   daemonset.apps/kube-flannel-ds-arm       0         0         0       0            0           beta.kubernetes.io/arch=arm       13m
    kube-system   daemonset.apps/kube-flannel-ds-arm64     0         0         0       0            0           beta.kubernetes.io/arch=arm64     13m
    kube-system   daemonset.apps/kube-flannel-ds-ppc64le   0         0         0       0            0           beta.kubernetes.io/arch=ppc64le   13m
    kube-system   daemonset.apps/kube-flannel-ds-s390x     0         0         0       0            0           beta.kubernetes.io/arch=s390x     13m
    kube-system   daemonset.apps/kube-proxy                1         1         1       1            1           beta.kubernetes.io/os=linux       25m
    
    NAMESPACE     NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
    kube-system   deployment.apps/coredns   2/2     2            2           25m
    
    NAMESPACE     NAME                                 DESIRED   CURRENT   READY   AGE
    kube-system   replicaset.apps/coredns-5c98db65d4   2         2         2       25m
    

### Run interactive container

You can do most anything in Kubernetes that you can do in Docker. E.g. run a simple interactive container:

    kubectl run busybox -i --tty --image=busybox --restart=Never --rm -- sh

    / # 
    / # busybox | head -1
    BusyBox v1.31.0 (2019-07-16 01:13:11 UTC) multi-call binary.
    
### Etherpad example

You can put as many Kube objects as you want in a YAML file and "apply" it all as one big chunk.

For example, take a look at mpackard-etherpad.yml.

    vim mpackard-etherpad.yml

If you're on a shared system, change all the "mpackard" to your name. Note there is Deployment and a Service section. The Deployment indicates what container(s) to run, and the Service creates a network route to the containers.

    cat mpackard-etherpad.yml

Deploy it:

    kubectl apply -f mpackard-etherpad.yml

See if it's running:

    kubectl get pod,deployment,service
    NAME                            READY   STATUS              RESTARTS   AGE
    pod/etherpad-55d84f9ffc-xjdlc   0/1     ContainerCreating   0          14s

    NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.extensions/etherpad   0/1     1            0           15s

    NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
    service/etherpad     NodePort    10.109.117.107   <none>        80:31763/TCP   14s
    service/kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        10m

**Note the external port 31763 of the etherpad service.**

Point your browser at your vm and that port number, e.g. http://129.114.104.121:31763

Now take it down:

    kubectl delete -f etherpad.yml 
    deployment.apps "etherpad" deleted
    service "etherpad" deleted

Edit the yml file and change "replicas" to 2, then save and re-apply.

    kubectl apply -f mpackard-etherpad.yml
    kubectl get pods

    NAME                            READY   STATUS              RESTARTS   AGE
    pod/mpackard-etherpad-5bd7855697-4vgcb   1/1     Running   0          5s
    pod/mpackard-etherpad-5bd7855697-wcfw4   1/1     Running   0          144m

Hit the URL again, create an etherpad, and hit refresh a couple times. What happens? 

#### Discussion Questions

- Why do you think the etherpad fails 50% of the time?

## Storage

Kube has many ways to handle persistent storage for your pods.

- persisten volume claims 
- more traditional file mounting (nfs, etc)

### Volume example

Hopefully you have a working local NFS server from the Ansible step.

    ls /kubedata/nginx
    enterprise.jpg index.html

This is going to get mounted inside our container so that nginx web server can serve it.

Take a look at mpackard-nginx.html. Change the "mpackard" to your username. Note the addition of the Volume and Volumemounts section.

Apply it:

    kubectl apply -f mpackard-nginx.html

Check for running:


    kubectl get pods,deployments,services
    NAME                                     READY   STATUS    RESTARTS   AGE
    pod/mpackard-nginx-688877c6f9-b99d7      1/1     Running   0          16m
    pod/mpackard-nginx-688877c6f9-ksdp4      1/1     Running   0          16m

    NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.extensions/mpackard-nginx      2/2     2            2           16m

    NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    service/mpackard-nginx      NodePort    10.97.84.239    <none>        80:31523/TCP     16m

Now point your browser at your IP & port (31523 here). Hit refresh a few times. Even though there are 2 nginx pods, they are serving the same data.


#### Discussion Questions

- What issues would you have with read-write data (e.g. if you were hosting a database)?



## Kubernetes Reset (destroys all current data)

**Caution** 

If you really need to reset Kubernetes and start over with new install:

    kubeadm reset -f
    systemctl stop kubelet
    rm -rf /etc/kubernetes /var/lib/kubelet /var/lib/etcd

Then re-run the playbook.

Previous: [Ansible](ansible.md) | Top: [Course Overview](../../index.md)

