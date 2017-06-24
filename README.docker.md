# CakePHP Application Skeleton With Docker

A skeleton for creating applications with [CakePHP](http://cakephp.org) 3.x and [Docker-Compose](https://docs.docker.com/compose).

The framework source code can be found here: [cakephp/cakephp](https://github.com/cakephp/cakephp).

The Dockerfiles can be found [here](https://hub.docker.com/r/cakephpfr/3.x/)

## Installation

1. Download [Git](https://git-scm.com/downloads).
2. Install docker on your computer with the [official doc](https://docs.docker.com/engine/installation/#installation).
3. Run the following.

```bash
# get an empty app
git clone git@github.com:cakewebsites/app.git --branch gandi
cd app
# remove history from app (not necessary for your project)
rm -rf .git
# install all dependencies
composer install --prefer-dist
# run all containers
docker-compose up -d
```

You should see the welcome page in your webbrowser at the address http://MACHINE_IP:NGINX_PORT (ie. http://192.168.99.100:32779).

To get the Nginx Port, run `docker-compose ps` to know on which port the containers are running. You'll get something like:

|  Name       |    Command            | State  |       Ports
| ------------|-----------------------|--------|-------------------------------
| app_db_1    | /entrypoint.sh mysqld | Up     | 0.0.0.0:32788->3306/tcp
| app_nginx_1 | nginx -g daemon off;  | Up     | 443/tcp, 0.0.0.0:32789->80/tcp
| app_php_1   | php-fpm               | Up     | 9000/tcp

So you can see that nginx is running on 32789's port in my case.

A reminder to get the MACHINE_IP, run `docker-machine ip default`. Returns in my case `192.168.99.100`

## Common dev tasks, usage of `composer` and `cake` console.

**For Composer**

Install the `composer.phar`  at the root of your repository with this command:

```bash
docker-compose run --rm php curl -s https://getcomposer.org/installer | php
```

And to install all dependancies for your project, just run:

```bash
docker-compose run --rm php ./composer.phar install
```

**For database migrations**

To create your tables. You can use the [Migrations Cakephp plugin](https://github.com/cakephp/migrations):

```bash
docker-compose run --rm php ./bin/cake migrations migrate
```

**To Test your application**

To test your app with [PHPUnit](https://phpunit.de/) (must be installed with composer):

```bash
docker-compose run --rm php ./vendor/bin/phpunit
```

**To Check your application code standards**

To test your app with [Codesniffer CakePHP plugin](https://github.com/cakephp/cakephp-codesniffer) (must be installed with composer):

```bash
// add the cakephp code standards to the codesniffer
docker-compose run --rm php ./vendor/bin/phpcs --config-set installed_paths vendor/cakephp/cakephp-codesniffer
// sniff the app
docker-compose run --rm php ./vendor/bin/phpcs --standard=CakePHP /path/to/code
```

**To enable DebugKit**

Copy the assets with:
```bash
docker-compose run --rm php ./bin/cake plugin assets copy
```
You should then see DebugKit in action.

## Configuration

Read and edit `docker-compose.yml` environment variables and any other configuration relevant for your application.
