# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "trusty64"
  config.vm.hostname = "apm-server"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "private_network", ip: "192.168.33.100"

  config.vm.synced_folder ".", "/var/www"

  config.vm.provider "virtualbox" do |vb|
     vb.name = "apm-server"
     vb.memory = "1024"
  end

  config.vm.provision "shell", inline: <<-SHELL
     sudo apt-get install software-properties-common
     sudo apt-add-repository ppa:ansible/ansible
     sudo apt-get update
     sudo apt-get install -y git ansible
     sudo git clone https://github.com/aossama/apm.git /opt/apm
     sudo -s su - vagrant -c 'ssh-keygen -f ~/.ssh/id_rsa -N "" && cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys'
     ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i /opt/apm/inventory/hosts /opt/apm/base.yml
  SHELL
end
