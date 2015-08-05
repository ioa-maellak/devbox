# A virtual LAMP stack for local development of web projects

## Prerequisites

This code provides a guest devevelopment box (ubuntu/trusty64) on your local
machine automatically configured as a LAMP server. To use the code provided you
need to have the following software intall on your machine:

  - [vagrant](https://www.vagrantup.com/downloads.html)
  - [ansible](http://www.ansible.com/home)
  - [virtualbox](https://www.virtualbox.org/)

On ubuntu hosts these packages are in official repositories and you can
install them the usual way:
```
sudo apt-get install vagrant ansible virtualbox
```

For Mac OS X hosts see how to install [homebrew](http://brew.sh/).

For windows hosts the software is available and there are guides online for
getting vagrant, ansible and virualbox working. Consider however using an operating
system that is friendlier for developemnt (download
[ubuntu](http://www.ubuntu.com/download/desktop)).

## Usage

The best way to use this repository is to fork it and tailor it to your
particular use case (see [configuration](#configuration) below). Follow
[these instructions](https://help.github.com/articles/fork-a-repo/) to
fork the repo and configure git to sync with it.

To create the guest server and automatically configure it do:
```
cd dev-box
vagrant up
```

Vagrant will first download ubuntu trusty (it takes a few minutes the first
time) and then will provision the guest server automatically according to
`playbook.yml`.

To get additional vagrant options you can use the `vagrant` command on the
directory that you cloned your repository.

The guest server will automotically be configured as a LAMP server utilising the
following sotfware:

  - PHP 5.6
  - Apache 2.4
  - Mysql 5.6
  - Composer
  - Drush 8
  - git

## Configuration

### Vagrant

The guest server links back to your local machine in two ways:

  - it forwards guest port `80` to host port `8080` of localhost. That means
    that you can visit the guest webserver by visiting `http://localhost:8080`
    on your browser,
  - it syncs a guest directory with a local directory so you can edit files in
    your local directory and automatically have the files changed on the guest
    server directory. If you have cloned the repository in `~/dev-box` then
    `~/data` is the directory in sync with server's `/var/www/html`.

These configuration options are set on `Vagrantfile` and can be changed
according to a particular use case.

### Ansible

Once the sercer is provisioned software is installed and databases are
created. You can control that behaviour by changing the values of the variables
that are included in `variables.yml` which are read by Ansible.

Ansible uses the YAML configuration language. Please take a look at
[YAML documentation](http://www.yaml.org/spec/1.2/spec.html) if you want to
learn more.

## Credits

This code is developed and maintained by Tassos Koutlas for the Ioannina FOSS
Unit of Excellence (ioa-maellak). It is distributed with a EUPL v1.1 open source
software license.
