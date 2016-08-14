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
        sudo apt-get install git -y
        echo 'export ANSIBLE_LIBRARY=/home/vagrant/thirdparty_modules' >> /home/vagrant/.profile
        if [ ! -d /home/vagrant/thirdparty_modules/win_dsc ]; then
          git clone https://github.com/trondhindenes/Ansible-win_dsc.git /home/vagrant/thirdparty_modules/win_dsc
        fi;
    "
    control.vm.provision "file", source: "config", destination: "~/ansible"
  end

  config.vm.define "web01" do |web01|
    # web01.vm.box = "mwrock/Windows2016"
    web01.vm.box = "jptoto/Windows2012R2"
    web01.vm.hostname = "win-web01"
    web01.vm.communicator = :winrm
    web01.winrm.username = "vagrant"
    web01.winrm.password = "vagrant"
    web01.vm.network "private_network", ip: "172.28.128.10"
    web01.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
    config.vm.provision :shell do |shell|
      shell.path = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
    end
    config.vm.provision :shell, inline: "
    if ($PSVersionTable.PSVersion.Major -eq 4)
    {
      choco install powershell 5 -y
      shutdown /r /t 0
    }
    "
  end
end

# ansible web -i ~/ansibleconfig/inventory.yml -m win_ping
