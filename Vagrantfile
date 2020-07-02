# Deploys a Single-Master Kubernetes Cluster with an Arbitrary Number of Nodes
#
# Based on: "Kubernetes Setup Using Ansible and Vagrant"
# https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/

BOX_NAME = "ubuntu/bionic64"

MASTER_CPUS = 2
MASTER_MEMORY = 1024

NODES = 1
NODE_CPUS = 4
NODE_MEMORY = 8096

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end

    config.vm.define "master", primary: true do |master|
        hostname = 'master'
        master.vm.box = BOX_NAME
        master.vm.network "private_network", ip: "192.168.56.129"
        master.vm.hostname = hostname
        master.vm.provision "file", source: "files/docker_daemon.json", destination: "docker_daemon.json"
        master.vm.synced_folder ".", "/vagrant"
        master.vm.provider :virtualbox do |vb|
          vb.gui = false
          vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
          # Be sure to enable I/O APIC for virtual machines that you intend to use in 64-bit mode.
          # This is especially true for 64-bit Windows VMs:
          # https://www.virtualbox.org/manual/ch03.html#intro-64bitguests
          vb.customize ["modifyvm", :id, "--ioapic", "on"]
          vb.customize ["modifyvm", :id, "--cpus", MASTER_CPUS]
          vb.customize ["modifyvm", :id, "--memory", MASTER_MEMORY]
          vb.customize ["modifyvm", :id, "--name", hostname]
        end
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
        if File.exist?('playbooks/join_command.sh')
          master.trigger.after :destroy do |trigger|
            trigger.name = "Remove join_command.sh"
            trigger.run = { inline: "rm -v playbooks/join_command.sh" }
          end
        end
    end

    if NODES >= 1 then
      (1..NODES).each do |i|
          hostname = "node%02d" % i
          config.vm.define hostname do |node|
              node.vm.box = BOX_NAME
              node.vm.network "private_network", ip: "192.168.56.#{i + 129}"
              node.vm.hostname = hostname
              node.vm.synced_folder ".", "/vagrant"
              node.vm.provider :virtualbox do |vb|
                vb.gui = false
                vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
                vb.customize ["modifyvm", :id, "--ioapic", "on"]
                vb.customize ["modifyvm", :id, "--cpus", NODE_CPUS]
                vb.customize ["modifyvm", :id, "--memory", NODE_MEMORY]
                vb.customize ["modifyvm", :id, "--name", hostname]
              end
              node.vm.provision "ansible_local" do |ansible|
                  ansible.playbook = "playbooks/kubernetes-node.yml"
              end
          end
      end
    end
end
