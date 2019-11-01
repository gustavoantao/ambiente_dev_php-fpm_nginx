# Lightweight dev environment for php

### Uses
- Docker
- Docker-compose

### Provides

- One Ngnix Server to route the requests for your apps
- One php-fpm (7.3) container for all apps
### Customization

The php image used is available with a bunch of php-extensions enabled out of the box. If you want to add or remove some of these extensions edit the Dockerfile in `/images/php/<version>` folder.

# How to use

Read the doc at `composer` folder. But is basically enter the composer directory and run a `docker-compose up` command.

Enjoy!!