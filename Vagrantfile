# Devbox Vagrant file.
#
# Look at default.config.yml for setting configuration parameters on this file.
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Create devbox with vagrant parameters
  config.vm.define "devbox" do |devbox|
    devbox.vm.box = "ubuntu/xenial64"
    devbox.vm.network "public_network", ip: "192.168.1.10"
    devbox.vm.network "forwarded_port", guest: "80", host: "8000"
    devbox.vm.synced_folder "~/projects", "/home/ubuntu/projects",
      owner: "ubuntu", group: "www-data", mount_options: ["dmode=775,fmode=664"]
  end

  # Virtualbox parameters
  config.vm.provider "virtualbox" do |vb|
    vb.name = "devbox"
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  # Workaround for ubuntu/xenial not having /usr/bin/python
  config.vm.provision "shell",
    inline: "if [[ ! -f /usr/bin/python ]]; then sudo ln -s /usr/bin/python3 /usr/bin/python; fi"

  # Provision with Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
