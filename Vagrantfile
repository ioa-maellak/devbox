# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Create devbox with vagrant parameters
  config.vm.define "devbox" do |devbox|
    devbox.vm.box = "ubuntu/xenial64"
    devbox.vm.network "forwarded_port", guest: 80, host: 8080
    devbox.vm.synced_folder "../mynewapp", "/var/www/html/mynewapp"
  end

  # Virtualbox parameters
  config.vm.provider "virtualbox" do |vb|
    vb.name = "devbox"
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  # Workaround for ubuntu/xenial not having /usr/bin/python
  config.vm.provision "shell" do |shell|
    shell.inline = "if [[ ! -f /usr/bin/python ]]; then sudo ln -s /usr/bin/python3 /usr/bin/python; fi"
  end
  
  # Provision with Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
