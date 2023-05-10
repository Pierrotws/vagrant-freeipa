# -*- mode: ruby -*-
# vi: set ft=ruby :

IMAGE_NAME = "centos/7"

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 4
  end
  #config.vbguest.auto_update = false

  config.vm.define "idm-server" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.hostname = "idm-server.my.domain"
    master.vm.network "private_network", ip: "192.168.56.20"
    master.vm.provision "ansible" do |ansible|
      ansible.groups = {
        "ipaservers" => ["idm-server"]
      }
      ansible.playbook = "provisioning/preparation-server.yml"
    end
  end

  config.vm.define "idm-node1" do |node|
    node.vm.box = IMAGE_NAME
    node.vm.hostname = "idm-node1.my.domain"
    node.vm.network "private_network", ip: "192.168.56.21"
    node.vm.provision "ansible" do |ansible|
      ansible.groups = {
        "ipaservers" => ["idm-server"],
        "ipaclients" => ["idm-node1"]
      }
      ansible.playbook = "provisioning/preparation-client.yml"
    end
  end
end
