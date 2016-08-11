# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "control" do |control|
    control.vm.box = "ubuntu/trusty64"
    control.vm.hostname = "ubuntu-control"
    control.vm.network "private_network", type: "dhcp"
    control.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
    end
    control.vm.provision "shell", inline: "
        sudo apt-get install software-properties-common -y
        sudo apt-add-repository ppa:ansible/ansible -y
        sudo apt-get update
        sudo apt-get install ansible python-pip -y
        sudo pip install pywinrm
        sudo pip install xmltodict
        sudo apt-get install tree -y
    "
    control.vm.provision "file", source: "config", destination: "~/ansible"
  end

  config.vm.define "web01" do |web01|
    web01.vm.box = "MattHodge/Win2012R2WMF5Min"
    web01.vm.hostname = "win-web01"
    web01.vm.communicator = :winrm
    web01.winrm.username = "vagrant"
    web01.winrm.password = "vagrant"
    web01.vm.network "private_network", type: "dhcp"
    web01.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
  end
end

# ansible web -i ~/ansibleconfig/inventory.yml -m win_ping
