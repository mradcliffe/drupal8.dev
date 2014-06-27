drupal8.dev
===========

Virtual Machine for drupal 8 development (VirtualBox 4.3)

## Installation

### 1. Clone this repo and Drupal 8.
```bash
$ git clone https://github.com/mradcliffe/drupal8.dev.git
$ cd drupal8.dev
$ mkdir www
$ cd www
$ git clone --branch 8.x http://git.drupal.org/project/drupal.git drupal8.dev
$ cd ..
$ vagrant up
```

### 2. Add VM to hosts file and flush DNS cache.
```bash
  sudo echo "192.168.56.101 drupal8.pgsql.dev drupal8.mysql.dev" >> /etc/hosts
```

### 4. Install Drupal 8
- Use `drush` or [your browser](http://drupal8.mysql.dev)

## Miscellaneous

### Tools on vm
* composer
* drush for drupal 8
* xdebug
* vim

### Database credentials
* Name: drupal8
* User: drupal8
* Pass: drupal8

## Minimum requirements
* Cygwin (Windows only)
* Git
* VirtualBox 4.3.x
* Vagrant 1.5.x
