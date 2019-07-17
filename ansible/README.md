# Single-Node Kubernetes on a Jetstream Instance

After running this playbook hopefully you will have a viable 1-node Kubernetes deployment.

Requires Ubuntu 18 + Docker Jetstream image.

Run as user (not root):

    ansible-playbook kube.yml

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
    

To launch an interactive busybox container:

    kubectl run busy -i --tty --image=busybox --restart=Never --rm -- sh
    
    
