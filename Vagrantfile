# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Create devbox with vagrant parameters
  config.vm.define "devbox" do |devbox|
    devbox.vm.box = "ubuntu/xenial64"
    devbox.vm.network "forwarded_port", guest: 80, host: 8080
    devbox.vm.synced_folder "~/projects", "/home/ubuntu/projects",
      owner: "ubuntu", group: "www-data", mount_options: ["dmode=775,fmode=664"]
  end

  # Virtualbox parameters
  config.vm.provider "virtualbox" do |vb|
    vb.name = "devbox"
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["storagectl", :id, "--name", "SCSI", "--hostiocache", "on"]
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
