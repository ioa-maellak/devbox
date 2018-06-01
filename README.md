# Devbox - Virtual LAMP server

## Prerequisites

This code provides a virtual devevelopment box (ubuntu/xenial64) on your local
machine automatically configured as a LAMP server. Using the LAMP stack you can
deploy projects without worrying about setting up your computer as a development
environment.

Devbox works best with the following software versions:

  - Vagrant >= 2.1.1
  - Ansible >= 2.5
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
[guides online for getting vagrant, ansible and virualbox working](https://www.jeffgeerling.com/blog/2017/using-ansible-through-windows-10s-subsystem-linux). Consider
however using an operating system that is friendlier for developemnt such as
[Ubuntu](http://www.ubuntu.com/download/desktop) as it will make your life
easier in other aspects of development.

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

You can login to the devbox with the following command:

```
vagrant ssh
```

Happy coding!

## Configuration

The development environment can be customised via the `config.yml` file and the
variables contained within. 

### Configuring a remote user

The `remote_user` variable is used to set the user in the guest machine. The
default user in Vagrant is ubuntu and is reccomended to be left as is.

### Configuring an RSA key

The `rsa_key` variable accepts as input a path from the host machine which
should resolve to an rsa key. It is used to authenticate in github and should be
added there prior to any git related operations in the development server.

### Configuring the server stack

The `config_roles` variable is used to customise the software stack configured
in the server. Comment out roles not needed.

### Configuring your projects

Vagrant allows to share directories between the host and the guest
systems. Devbox takes advantage of that feature to expose local projects which
can then be served through the LAMP stack.

Devbox expects a `projects` folder to exist in the same contains directory with
devbox:

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

For each project the following parameters can be set:

- domain (string): The URI to access project.
- docroot (string) The path of the project in devbox (user `{{ remote_user }}`
  for indepedence
- db_user (string): The name of the database user to be used (root will be used
  if empty)
- db_pass (string): The passwrd of the database user to be used (root will be used if empty)
- db_name (string): The database name to be used (if empty database creation is skipped)
- tech (string): The technology used in the project. Options include:
    - hugo
	- drupal
	- symfony

### Configuring additional software to be added

The `additional_packages` variable allows additional software to be included in
the main build. It is intended for smaller packages that do not require
customisation. If more software with significant customisation is needed
consider to open a pull request.

### Configuring Vagrant

Devbox offers a complete development environment out of the box. Vagrant is
controled through a `Vagrantfile` bunlded at the root of the repository. Within
that file there are parameters about:

  - vagrant options about the base image, synchronising folders, network, etc
  - virtual machine options about memory, etc
  - provision options about ansible

Please follow through [vagrant documentation](https://www.vagrantup.com/docs/)
for a more thorough understanding of how it can be tweaked.

## Usage

### Using Vagrant

To connect to the VM use the following:

```
vagrant ssh
```

For changes in `config.yml` to take effect use:

```
vagrant provision
```

For changes in `Vagrantfile` to take effect vagrant configuration need to be roloaded:

```
varnish reload
```

Access help via:

```
vagrant help
```

### Accessing projects

First, override DNS to point to the domain of your project (e.g. domain is set
to `myapp.local`). Add the folloring entry in your `/etc/hosts` file:

```
127.0.0.1    myapp.local
```

Then point your browser to the domain in port 8000:

```
http://myapp.local:8080
```

You should see the homepage of your project.

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
