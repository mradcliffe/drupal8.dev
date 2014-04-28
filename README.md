drupal8.dev
===========

Virtual Machine for drupal 8 development (VirtualBox 4.3)

1.- Clone this repo
```bash
$ git clone https://github.com/mradcliffe/drupal8.dev.git
$ cd drupal8.dev
$ vagrant up
```

2.- Add VM to hosts file and flush DNS cache.
```bash
  192.168.56.101 drupal8.pgsql.dev drupal8.mysql.dev
```

3.- Clone Drupal 8 repo
```bash
$ vagrant ssh
$ cd /vagrant/www
$ git clone --branch 8.x http://git.drupal.org/project/drupal.git drupal8.mysql.dev
$ git clone --branch 8.x http://git.drupal.org/project/drupal.git drupal8.pgsql.dev
```

4.- Install Drupal 8
* Using drush
```bash
$ drush si standard --db-url=mysql://drupal:drupal@localhost/drupal --site-name=drupal8.mysql.dev --account-name=admin --account-pass=admin --account-mail=[user-email]
```

* Using the GUI accessing http://drupal8.*.dev

#### Tools on vm
* composer
* drush for drupal 8
* xdebug
* vim

#### Database Credentials
* Name: drupal8
* User: drupal8
* Pass: drupal8

#### Minimum requirements
* Git
* VirtualBox 4.3.x
* Vagrant 1.5.x
* Cygwin (for Windows)
