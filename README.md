## Overview
This repository is for creating my setting on centOS container.

## Preparation for creating docker OS
If you haven't host machine for docker, please install by following the procedure
```
# Create docker container on virtualbox
docker-machine create --driver virtualbox test-docker

# Get the environment commands for your new VM
docker-machine env test-docker

# Connect your shell to the new machine
eval "$(docker-machine env test-docker)"
```

## Preparation in docker OS
```
# Login
docker-machine ssh test-docker

# Install python
tce-load -wi python

# Install pip
curl https://bootstrap.pypa.io/get-pip.py | sudo python -

# Install docker.py
sudo pip install docker-py

# Create symbolic link
sudo ln -s /usr/local/bin/python /usr/bin/python
```

## Execution
```
eval "$(docker-machine env test-docker)"
ansible-playbook site.yml
```

## Confirmation
```
# Check the process
docker ps

## Confirm in docker OS
docker exec -it test-container /bin/sh
```

## Rollback
```
# Remove docker container
docker rm -f test-container

# Remove docker host
docker-machine rm test-docker
```

## Memo
* Make sure ip address described in ssh_config is correct or not. you can see the address by `docker-machine env test-docker`
