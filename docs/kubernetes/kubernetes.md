## Kubernetes

Kubernetes is an orchestration tool that allows management of conainers across multiple nodes.

First we'll install it on our instance using Ansible.

## Single-Node Kubernetes on a Jetstream Instance using Ansible

After running this playbook hopefully you will have a viable 1-node Kubernetes deployment.

Requires Ubuntu 18 + Docker Jetstream image.

Run as user (not root):

    ansible-playbook ansible-kube-playbook.yml

Once it is finished, you can check that everything is up:

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

    kubectl run busybox -i --tty --image=busybox--restart=Never --rm -- bash

    / # 
    / # busybox | head -1
    BusyBox v1.31.0 (2019-07-16 01:13:11 UTC) multi-call binary.
    

### Kube object files

Kubernetes has different types of objects. You can put as many as you want in a file and "apply" it all as one big chunk.

For example, Etherpad.

    cat etherpad.yml

Deploy it:

    kubectl apply -f etherpad.yml

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

### Remove example

To take down the objects:

    kubectl delete -f etherpad.yml 
    deployment.apps "etherpad" deleted
    service "etherpad" deleted


### Storage

Kube has many ways to handle persistent storage for your pods.

### Trimmomatic

    docker pull jurrutia/dockercomposepipeline_trimmomatic
    docker pull jurrutia/dockercomposepipeline_star
    docker pull jurrutia/dockercomposepipeline_multiqc

### Kubernetes Reset (destroys all current data)

To reset Kubernetes and start over with new Kube install:

    kubeadm reset -f
    systemctl stop kubelet
    rm -rf /etc/kubernetes /var/lib/kubelet /var/lib/etcd

Then re-run the playbook.

