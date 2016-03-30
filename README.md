drupal8.dev
===========

Virtual Machine for drupal 8 development (VirtualBox 4.3).

## Installation

PuPHPet is still going rapid development. If you are updating your virtual machine, it would be best to destroy and re-up.

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
* VirtualBox 4.3 or greater
* Vagrant 1.8 or greater

## Troubleshooting

* Running Composer or using [Composer Manager](https://drupal.org/project/composer_manager) from within inside the VM not compatible with rsync `--delete` parameter. I recommend to try to setup so you can run Composer outside the VM and rsync files into it. This may mess with a git checkout of core.
