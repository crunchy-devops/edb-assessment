# Tutorial

## Prerequisites on ubuntu 20.04
```shell
sudo apt-get update  # update links to repos
sudo apt-get -y install git wget htop iotop iftop # install git and monitoring tools
sudo apt-get -y install python3 python3-venv # install python3 and virtualenv
sudo apt-get -y install build-essential   # need for installing docker-compose
sudo apt-get -y install python3-dev libxml2-dev libxslt-dev libffi-dev # need for installing docker-compose
htop  # check your vm config
Crtl-c  # exit
````

## Django set up on virtualenv on Ubuntu 20.04
```shell
cd   # you should be on your home directory
git clone  https://github.com/crunchy-devops/edb-assessment.git
cd edb-assessment/django-polls
python3 -m venv venv  # set up the module venv in the directory venv
source venv/bin/activate  # activate the virtualenv python
pip3 install wheel  # set for permissions purpose
pip3 install django  # install django 
python3 -m django --version  # check version number
python3 manage.py migrate  # create database schema
python3 manage.py createsuperuser # set superuser username and password
python3 manage.py runserver 0:8000 # sanity test and enter your first poll 
```