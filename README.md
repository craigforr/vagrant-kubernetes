# Vagrant Kubernetes Cluster

Kubernetes cluster deployment with a single control plane node and an arbitrary number of nodes, using Vagrant (VirtualBox provider, Ansible provisioner).

## What's in the Box:

The default configuration will build a 4 node cluster --- a master and 3 workers:

```
$ kubectl get nodes
NAME     STATUS   ROLES    AGE    VERSION
master   Ready    master   176m   v1.17.0
node01   Ready    <none>   115m   v1.17.0
node02   Ready    <none>   79m    v1.17.0
node03   Ready    <none>   74m    v1.17.0
```

## Prerequisites

You will need the following installed:

- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) (Optional)
- [Git](https://git-scm.com/) (Optional)

## Getting Started

1. Download the zip file or `git clone` this repository:
```bash
git clone https://github.com/craigforr/vagrant-kubernetes.git
```
2. Change into the project directory and run Vagrant:
```bash
cd vagrant-kubernetes
vagrant up
```
3. Once the cluster is built, you can either run kubectl against the master from your local system or SSH into the master, as follows:
```bash
vagrant ssh
```

<!-- EOF -->
