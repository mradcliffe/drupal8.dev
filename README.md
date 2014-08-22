drupal8.dev
===========

Virtual Machine for drupal 8 development (VirtualBox 4.3)

## Installation

### 1. Clone this repo and Drupal 8.

Instructions assume a unix-like machine such as OS X or Linux.

```bash
git clone https://github.com/mradcliffe/drupal8.dev.git
cd drupal8.dev
cd www
git clone --branch 8.0.x http://git.drupal.org/project/drupal.git drupal8.dev
rsync -rtlDPvc tmp/ drupal8.dev/
cd ..
vagrant up
```

### 2. Add VM to hosts file and flush DNS cache.

- Add `192.168.56.101 drupal8.sqlite.dev drupal8.pgsql.dev drupal8.mysql.dev` to /etc/hosts on the Host machine, and flush cache.

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

## Troubleshooting

* Issue with mailcatcher puppet and sqlite puppet reositories being incompatible?
* mradcliffe/puppetlabs-postgresql needs to be tweaked a bit.
