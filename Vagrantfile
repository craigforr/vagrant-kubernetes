# Deploys a Single-Master Kubernetes Cluster with an Arbitrary Number of Nodes
#
# Base on: "Kubernetes Setup Using Ansible and Vagrant"
# https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/

BOX_NAME = "bento/ubuntu-16.04"
# BOX_NAME = "ubuntu/xenial64"
NODES = 3

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
    end

    config.vm.define "master", primary: true do |master|
        master.vm.box = BOX_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "master"
        master.vm.synced_folder ".", "/vagrant"
        master.vm.provision "ansible_local" do |ansible|
            ansible.compatibility_mode  = "2.0"
            ansible.playbook            = "playbooks/kubernetes-master.yml"
            ansible.verbose             = false
        end
        master.vm.provision "ansible_local" do |ansible|
            ansible.compatibility_mode  = "2.0"
            ansible.playbook            = "playbooks/kubernetes-master-extras.yml"
            ansible.verbose             = false
        end
    end

    (1..NODES).each do |i|
        hostname = "node%02d" % i
        config.vm.define hostname do |node|
            node.vm.box = BOX_NAME
            node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
            node.vm.hostname = hostname
            node.vm.synced_folder ".", "/vagrant"
            node.vm.provision "ansible_local" do |ansible|
                ansible.playbook = "playbooks/kubernetes-node.yml"
            end
        end
    end
end
