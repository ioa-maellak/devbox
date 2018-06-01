# Devbox - Virtual LAMP server

## Prerequisites

This code provides a virtual devevelopment box (ubuntu/xenial64) on your local
machine automatically configured as a LAMP server. Using the LAMP stack you can
deploy projects without worrying about setting up your computer as a development
environment.

Devbox works best with the following software versions:

  - Vagrant > 1.8.1
  - Ansible > 2.2.2
  - VirtualBox
  - Ubuntu Xenial 16.04

On Ubuntu hosts these packages are in official repositories and you can
install them the usual way:

```
sudo apt-get install vagrant ansible virtualbox -y
```

Bare in mind that you might need to add additional PPA repositories to get
specific versions of the packages you need. For Mac OS X hosts see how to
install [homebrew](http://brew.sh/).

For windows hosts the software is available and there are
[guides online for getting vagrant, ansible and virualbox working](https://www.jeffgeerling.com/blog/2017/using-ansible-through-windows-10s-subsystem-linux).
Consider however using an operating system that is friendlier for developemnt
such as [Ubuntu](http://www.ubuntu.com/download/desktop) as it will make your
life easier in other aspects of development.

## Usage

To create the guest server and automatically configure it run the following
command:

```
git clone git@github.com:ioa-maellak/devbox.git && cd devbox
```

Next create a `config.yml` to configure your server like so:

```
cp default.config.yml config.yml
```

Finally run Vagrant:

```
vagrant up
```

Vagrant will first download Ubuntu (it takes a few minutes the first time) and
then will provision the virtual web server automatically according to
`playbook.yml`.

You can login to the devbox with the following command:

```
vagrant ssh
```

## Configuration

### Configuring projects

Vagrant allows to share directories between the host and the guest
systems. Devbox takes advantage of that feature to expose local projects which
can then be served through the LAMP stack.

Devbox expects a `projects` folder to exist in the same directory that contains
devbox like in the following structure:

```
.
├── devbox
└── projects
   ├── projectA
   ├── projectB
   └── projectC
```

All directories contained withing projects will be exposed in `~/projects` in
the guest system.

### Deploying projects

How projects are served through devbox is controled on the main `variables.yml`
file. In there you will find a `projects` array which you can add the following
configuration parameters to the projects:

  - domain: the URL you want to enter to the browser to access the project (e.g `http://myapp.local`)
  - docroot: the document root of the project (e.g `~/projects/projectA/web`)
  - db_user: the name of the user to add to the database and link to your
  project (leave blank for using the default root)
  - db_pass: the password of db_user (leave blank for using the default root
  password)
  - db_name: the name of the database to be used for the project
  - framework: the name of framework used (ie. php, laravel, symfony, magento, wordpress)

You need to reload varnish for new configuration to take effect:

```
varnish reload
```

### Accessing projects

Accessing projects from the host is a simple process that requires two steps:

  1. Add a DNS override on your host machine to point to the URL you provided as
  the domain in the `project` variable
  2. Point your browser to the URL at port 8080

The first step allows your host machine to know about devbox and associates its
IP with the URLs you provide. To do that add the following line on your `/etc/hosts`
file ([learn more about overriding DNS with /etc/hosts](http://bencane.com/2013/10/29/managing-dns-locally-with-etchosts/)):

```
127.0.0.1    myapp.local
```

Once that step is completed point your browser to the URL followed by port 8080
like so:

```
http://myapp.local:8080
```

You will be served a page from your project through the LAMP stack.

### Configuring devbox - Vagrant

Devbox aims to offer a complete development environment out of the box.If you
want to extend it then you would need to have a working knowledge ansible and
vagrant.

Vagrant is controled through a `Vagrantfile` bunlded at the root of the
repository. Within that file there are parameters about:

  - vagrant options about the base image, synchronising folders, network, etc
  - virtual machine options about memory, etc
  - provision options about ansible

Please follow through [vagrant documentation](https://www.vagrantup.com/docs/)
for a more thorough understanding of how it can be tweaked.

### Configuring devbox - Ansible

Ansible tasks are split into 4 primary roles:

 - **Common** - everything
 - **Apache** - tasks, variables and templates partaining to setting up Apache
 - **PHP** - tasks, variables and templates pertaining to setting up PHP
 - **MySQL** - tasks, variables and templates pertaining to setting up MySQL

Each role contains configuration for tasks, templates, variables, etc and it is
aiming to be a self-documenting standalone role. You can change the behaviour of
devbox by changing the files in those directories. Have a look at the next
section to understand how you can contribute to devbox.

## Contributions

Everyone is welcome to contribute. The most established way to do so in github
is to fork a project and create a pull request. Read this post on
[how to use pull requests on github](https://help.github.com/articles/using-pull-requests/).

## Credits

This code is developed and maintained by
[Tassos Koutlas](https://github.com/tassoskoutlas) for the Ioannina FOSS Unit of
Excellence ([ioa-maellak](https://github.com/ioa-maellak/)).

The code is distributed under the
[EUPL v1.1](http://ec.europa.eu/idabc/eupl.html) open source software license.
