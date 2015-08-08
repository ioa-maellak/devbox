# Devbox - Virtual LAMP server

## Prerequisites

This code provides a virtual devevelopment box (ubuntu/trusty64) on your local
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
cd devbox
vagrant up
```

Vagrant will first download ubuntu trusty (it takes a few minutes the first
time) and then will provision the virtual web server automatically according to
`playbook.yml`.

What you get at the end of this process is:
  - a virtual web server and database server that can host your web apps,
  - a default web app configured to be accessed from your local browser at
  `http://mynewapp.local:8080`,
  - a synced folder between your local filesystem and the virtual server to host
  your web app files,
  - an environment that can be extended to accomodate more web apps, databases
  and code repositories.

You will also need to let you local machine know which ip is associated with the
`mynewapp.local` domain. To do that add the following line on your `/etc/hosts`
file ([learn more about overriding DNS with /etc/hosts](http://bencane.com/2013/10/29/managing-dns-locally-with-etchosts/)):

```
127.0.0.1    mynewapp.local
```

To login to the virtual web server use:

```
vagrant ssh
```

The following section explores how the virtual web server works and how more
than one web app can be setup and served by it.

## Configuration

To add another web app to the virtual servers you will need to:
  1. edit the `Vagrantfile` and add new synced folder,
  2. add the domain and docroot of the web app in `variables.yml`
  3. reload the vagrant configuration with `vagrant reload`,
  4. provision the server again so that virtual hosts are created,
  5. edit `/etc/hosts` on your local machine and add the new web app domain on
     `127.0.0.1`

Have a look on `generic.vhost.j2` for exact implementation details for the
apache configuration of web app docroots.

Ansible uses the YAML configuration language. Please take a look at
[YAML documentation](http://www.yaml.org/spec/1.2/spec.html) if you want to
learn more.

## Guest server packages

The guest server will automotically be configured as a LAMP server utilising the
following sotfware:

  - PHP 5.6
  - Apache 2.4
  - Mysql 5.6
  - Composer
  - Drush 8
  - git

## Credits

This code is developed and maintained by
[Tassos Koutlas](https://github.com/tassoskoutlas) for the Ioannina FOSS Unit of
Excellence (ioa-maellak). Pull requests are welcome.

The code is distributed with a EUPL v1.1 open source software license.
