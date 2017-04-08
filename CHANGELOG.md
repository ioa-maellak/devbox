# Change Log

All changes to the project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

## [2.1.0] - 2017-04-08

### Added
- New project role to handle project specific configuration
- Added `php-mbstring`
- Changed host with new Ansible variable syntax (> 2.0)
- Splits several YAML files into multiple and adds include statements to
  individual `main.yml` files.

### Removed
- All configuration regarding projects (vhost and database) now resides with the
project role.


## [2.0.0] - 2017-04-06

### Added
- Split the code into roles to be more manageable
- Adds CHANGELOG.md
- Tags release 1.0.0

### Changes
- Changes to ansible code syntax to be compatible with Ansible 2.2.1
- Changes to vagrant code syntax to be compatible with Vagrant 1.9.2
- Changes to vagrant to use Ubuntu Xenial 16.04

## [1.0.0] - 2015-08-24

### Added
- First release of code
