# Docker-Pyload-Alpine
Your pyload container with Docker Compose.

## Description
Use Docker Compose to create and run a [Pyload](https://pyload.net/) container on Alpine Linux image.

## Install Docker
CentOS 7 :
```
yum install docker -y
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

Debian 9 :
```
#Install required packages
apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common

#Add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add -

#Add repo
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable"

#Update and install
apt-get update
apt-get install docker-ce docker-compose
```

## Run docker on server startup
CentOS 7 / Debian 9 :
```
systemctl enable docker
systemctl start docker
```

CentOS 6 / Debian 8 :
```
update-rc.d docker defaults
service docker start
```

## Build your Pylaod container
### Create Pyload user
CentOS :
```
useradd --comment "Pyload User" --create-home --shell /sbin/nologin pyload
```

Debian :
```
useradd --comment "Pyload User" --create-home --shell /user/sbin/nologin pyload
```

### Get files
Download and unzip files from repository :
```
cd /home/pyload
wget https://github.com/naabOmatic/Docker-Pyload-Alpine/archive/master.zip
unzip master.zip
cd Docker-Pyload-Alpine-master
```

You should have the following files :
* `.env` the only file you should modify
* `docker-compose.yml` used by docker-compose
* `Dockerfile-compose` build your pyload image
* `README.md` which you are reading right now

### Set variables
Edit .env file to meet your needs :
  * `MYUSER` : the user created just above
  * `MYGROUP` : the group created automaticaly (should be same as MYUSER)
  * `CONFIG` : path to keep your config file outside the container
  * `DOWNLOADS` : path to save your downloads outside the container

### Build your image
Run docker-compose to build your pyload image :
```
docker-compose build pyload
```

### Set permissions
Change owner of your config and downloads path :
```
chown -R pyload:pyload /home/pyload/
```

## First Run
Use the following command to run first pyload setup :
```
docker-compose run pyload
```
You can leave defaults options however you should configure at least :
* Username [User]: `pyload`
* Downloadfolder [Downloads]: `/app/pyload/Downloads`
* Server ([builtin], threaded, fastcgi, lightweight): `lightweight`

## Usage
Day to day usage :
* Run : `docker-compose up -d`
* Stop : `docker-compose stop`
* Debug : `docker-compose run pyload --debug`

## Known issues
When the container is not properly stopped, a PID file avoid the container to launch. You need to remove pyload.pid in your conf directory.

## Sources
https://docs.docker.com/engine/installation/linux/docker-ce/debian/
https://docs.docker.com/compose/install/#install-compose
