# stakkr-docker

Up and running with small Docker dev environments.

## Documentation

Full documentation can be found at [https://vessel.shippingdocker.com](https://vessel.shippingdocker.com).

## Install

Stakkr is just a small set of files that sets up a local Docker-based dev environment per project. There is nothing to install globally, except Docker itself!

This is all there is to using it:

```bash
composer require composer require stakkr/docker-symfony


# Run this once to initialize project
# Must run with "bash" until initialized
bash stakkr init

./stakkr start
```

Head to `http://localhost` in your browser and see your Laravel site!

ou can do this with this command:

    cp -R vendor/shipping-docker/vessel/docker-files/{vessel,docker-compose.yml,docker} .
    
and then you'll be able to install and continue as normal.
    
    
## Multiple Environments

Stakkr attempts to bind to port 80 and 3306 on your machine, so you can simply go to `http://localhost` in your browser.

However, if you run more than one instance of Vessel, you'll get an error when starting it; Each port can only be used once. To get around this, use a different port per project by setting the `APP_PORT` and `MYSQL_PORT` environment variables in one of two ways:

Within the `.env` file:

```
APP_PORT=8080
MYSQL_PORT=33060
```

Or when starting Stakkr:

```bash
APP_PORT=8080 MYSQL_PORT=33060 ./stakkr start
```

Then you can view your project at `http://localhost:8080` and access your database locally from port `33060`;


## Common Commands

Here's a list of built-in helpers you can use. Any command not defined in the `vessel` script will default to being passed to the `docker-compose` command. If not command is used, it will run `docker-compose ps` to list the running containers for this environment.

### Starting and Stopping Vessel

```bash
# Start the environment
./stakkr start

## This is equivalent to
./stakkr up -d

# Stop the environment
./stakkr stop

## This is equivalent to
./stakkr down
```

### Development

```bash
# Use composer
./stakkr composer <cmd>
./stakkr comp <cmd> # "comp" is a shortcut to "composer"

# Use artisan
./stakkr bin/console <cmd>
./stakkr console <cmd> # "console" is a shortcut to "bin/console"

# Run phpunit tests
./stakkr test

## Example: You can pass anything you would to phpunit to this as well
./stakkr test --filter=some.phpunit.filter
./stakkr test tests/Unit/SpecificTest.php


# Run npm
./stakkr npm <cmd>

## Example: install deps
./stakkr npm install

# Run yarn

./stakkr yarn <cmd>

## Example: install deps
./stakkr yarn install

# Run gulp
./stakkr gulp <cmd>
```

### Docker Commands

As mentioned, anything not recognized as a built-in command will be used as an argument for the `docker-compose` command. Here's a few handy tricks:

```bash
# Both will list currently running containers and their status
./stakkr
./stakkr ps

# Check log output of a container service
./stakkr logs # all container logs
./stakkr logs app # nginx | php logs
./stakkr logs mysql # mysql logs
./stakkr logs redis # redis logs

## Tail the logs to see output as it's generated
./stakkr logs -f # all logs
./stakkr logs -f app # nginx | php logs

# Start a bash shell inside of a container
# This is just like SSH'ing into a server
# Note that changes to a container made this way will **NOT** 
#   survive through stopping and starting the vessel environment
#   To install software or change server configuration, you'll need to
#     edit the Dockerfile and run: ./vessel build
./stakkr exec app bash
```


## What's included?

The aim of this project is simplicity. It includes:

* PHP 7.2
* MySQL 5.7
* Redis ([latest](https://hub.docker.com/_/redis/))
* NodeJS ([latest](https://hub.docker.com/_/node/)), with Yarn & Gulp

## How does this work?

If you're unfamiliar with Docker, try out this [Docker in Development](https://serversforhackers.com/s/docker-in-development) course, which explains important topics in how this is put together.

If you want to see how this workflow was developed, check out [Shipping Docker](https://serversforhackers.com/shipping-docker) and signup for the free course module which explains building this Docker workflow.

## Supported Systems

Stakkr requires Docker, and currently only works on Windows, Mac and Linux.

> Windows requires running Hyper-V.  Using Git Bash (MINGW64) and WSL are supported.  Native
  Windows is still under development.

| Mac           | Linux         | Windows |
| ------------- |:-------------:|:-------:|
| Install Docker on [Mac](https://docs.docker.com/docker-for-mac/install/) | Install Docker on [Debian](https://docs.docker.com/engine/installation/linux/docker-ce/debian/) | Install Docker on [Windows](https://docs.docker.com/docker-for-windows/install/) |
|       | Install Docker on [Ubuntu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/) | |
|       | Install Docker on [CentOS](https://docs.docker.com/engine/installation/linux/docker-ce/centos/) | |
