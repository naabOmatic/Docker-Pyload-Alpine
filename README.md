# Docker-Pyload-Alpine
Setup your pyload container with Docker Compose.

## Description
Use Docker Compose to create and run a pyload container on Alpine Linux image.

## Install
1. Install docker and docker-compose
2. Clone or download this repository
3. Create user and group (i.e. pyload)
4. Set variables in .env file to meet your needs :
  * MYUSER : the user created just above
  * MYGROUP : the group created just above
  * CONFIG : path to keep your config file outside the container
  * DOWNLOADS : path to save your downloads outside the container
5. Change owner of your config and downloads path if needed

## First Run
* First run : docker-compose run pyload

## Usage
* Run : docker-compose up -d
* Stop : docker-compose stop

## Known issues
* When the container is not properly stopped, a PID file avoid the container to launch. Please remove pyload.pid in your conf directory.
