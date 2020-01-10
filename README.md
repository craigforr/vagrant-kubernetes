# Vagrant Kubernetes Cluster

Kubernetes cluster deployment with a single control plane node and an arbitrary number of nodes, using Vagrant (VirtualBox provider, Ansible provisioner).

## What's in the Box

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
4. When you're done using the cluster, you can either stop the nodes or destroy the cluster:
- Power down all the nodes:
```
$ vagrant halt
==> node03: Attempting graceful shutdown of VM...
==> node02: Attempting graceful shutdown of VM...
==> node01: Attempting graceful shutdown of VM...
==> master: Attempting graceful shutdown of VM...
```
- Delete all the nodes:
```
$ vagrant destroy
# vagrant destroy --force
==> node03: Destroying VM and associated drives...
==> node02: Destroying VM and associated drives...
==> node01: Destroying VM and associated drives...
==> master: Destroying VM and associated drives...
==> master: Running action triggers after destroy ...
==> master: Running trigger: Remove join_command.sh...
    master: Running local: Inline script
    master: rm -v playbooks/join_command.sh
    master: VERBOSE: Performing the operation "Remove File" on target
    master: "/home/user/vagrant-kubernetes/playbooks/join_command.sh".

```

To see the status of the cluster nodes at any time, use `vagrant status`:

```
$ vagrant status
Current machine states:

master                    poweroff (virtualbox)
node01                    poweroff (virtualbox)
node02                    poweroff (virtualbox)
node03                    poweroff (virtualbox)
```


<!-- EOF -->
