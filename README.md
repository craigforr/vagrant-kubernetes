# Vagrant Kubernetes Cluster

Kubernetes cluster deployment with a single control plane node and an arbitrary number of nodes, using Vagrant (VirtualBox provider, Ansible provisioner).

The default configuration will build a 4 node cluster --- a master and 3 workers:

```
$ kubectl get nodes
NAME     STATUS   ROLES    AGE    VERSION
master   Ready    master   176m   v1.17.0
node01   Ready    <none>   115m   v1.17.0
node02   Ready    <none>   79m    v1.17.0
node03   Ready    <none>   74m    v1.17.0
```

<!-- EOF -->
