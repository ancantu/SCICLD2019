## Kubernetes

Kubernetes is an orchestration tool that allows management of conainers across multiple nodes.


### Install with Ansible 

Check out this repo on your instance.

    ansible SCICLD2019/ansible/kube.yml

See README.md there.


### Run interactive container


    kubectl run busybox -i --tty --image=busybox--restart=Never --rm -- bash


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


### Storage

Kube has many ways to handle persistent storage for your pods.

### Trimmomatic

    docker pull jurrutia/dockercomposepipeline_trimmomatic
    docker pull jurrutia/dockercomposepipeline_star
    docker pull jurrutia/dockercomposepipeline_multiqc


