# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # General vagrant parameters
  config.vm.box = "ubuntu/trusty64"
  config.vm.netwrok "forwarded_port", guest: 80, host: 8080
  config.vm.synced_folder "../data", "/var/www/html/"

  # Virtualbox parameters
  config.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "1024"]
   end
  
  # Provision with Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
