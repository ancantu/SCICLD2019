## Ansible

Ansible example.

Ansible uses "playbooks" to script actions to be performed on one or more destination hosts.

    vi ansible-kube-playbook.yml

## Single-Node Kubernetes on a Jetstream Instance using Ansible

After running this playbook hopefully you will have a viable 1-node Kubernetes deployment.

Requires Ubuntu 18 + Docker Jetstream image.

Run as user (not root):

    ansible-playbook ansible-kube-playbook.yml

Go ahead and run another one to set things up for later:

    ansible-playbook ansible-nfs-playbook.yml



Next: [Kubernetes](kubernetes.md) | Top: [Course Overview](../../index.md)


