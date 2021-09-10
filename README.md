# edb-assessment from scratch

## Configuration 
VM: Ubuntu 20.04  
RAM : 4 Gb    
3 Core

## Prerequisites on ubuntu 20.04
```shell
sudo apt-get update  # update links to repos
sudo apt-get -y install git wget htop iotop iftop # install git and monitoring tools
sudo apt-get -y install python3 python3-venv # install python3 and virtualenv
sudo apt-get -y install build-essential   # need for installing docker-compose
sudo apt-get -y install python3-dev libxml2-dev libxslt-dev libffi-dev # need for installing docker-compose
htop  # double check your vm config
Crtl-c  # exit
```

## Clone the repo  on the VM
```shell
cd   # you should be on your home directory
git clone  https://github.com/crunchy-devops/edb-assessment.git
```

## Install a virtualenv
```shell
python3 -m venv venv  # set up the module venv in the directory venv
source venv/bin/activate  # activate the virtualenv python
pip3 install wheel  # set for permissions purpose
```

## install docker 
```shell
pip3 install ansible # install ansible
pip3 install requests # extra packages
ansible --version # check the version number # should be the latest 2.11.2
ansible-playbook -i inventory_for_ubuntu install_docker_ubuntu.yml --limit local  # run the playbook for installing docker
docker version  # check 
# close your IDE and start again
cd
cd edb-assessment
source venv/bin/activate
docker ps # Your normal user should be able to start docker  
```

## Get a grip on the Poll app
Go to FIRST_STEP.md in django-polls

