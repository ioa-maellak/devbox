# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Create devbox with vagrant parameters
  config.vm.define "devbox" do |devbox|
    devbox.vm.box = "ubuntu/trusty64"
    devbox.vm.network "forwarded_port", guest: 80, host: 8080
    devbox.vm.synced_folder "../data", "/var/www/html/"
  end

  # Virtualbox parameters
  config.vm.provider "virtualbox" do |vb|
    vb.name = "devbox"
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  
  # Provision with Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
