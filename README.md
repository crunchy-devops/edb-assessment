# edb-assignment from scratch

## Configuration 
VM: Ubuntu 20.04  
RAM : 4 Gb    
3 Core  
IDE: Goland  JetBrains  
Some data are present in the data directory

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
git clone  https://github.com/crunchy-devops/edb-assignment.git
```

## Install a virtualenv
```shell
cd edb-assignment
python3 -m venv venv  # set up the module venv in the directory venv
source venv/bin/activate  # activate the virtualenv python
pip3 install wheel  # set for permissions purpose
```

## Install docker 
```shell
pip3 install ansible # install ansible
pip3 install requests # extra packages
ansible --version # check the version number # should be the latest 2.11.5
ansible-playbook -i inventory install_docker_ubuntu.yml --limit local  # run the playbook for installing docker
docker version  # check 
# close your IDE and start again for all changes take effect
cd
cd edb-assignment
source venv/bin/activate
docker ps # Your normal user should be able to start docker  
```

## Get a grip on the Poll app sqlite3 version
Follow FIRST_STEP.md in django-polls directory

## Get a grip on the Poll app docker/postgresql/docker version
Follow SECOND_STEP.md in django-polls-postgresql  directory

## Get a grip on jmeter testing (Docker version )
Follow README.md in jmeter directory   
Get a some performance measure  

## Install Kind
Follow README.md in kind directory

## Go to JMETER for k8s 
Follow JMETER-K8S.md in jmeter directory  
Get a some performance measure  

## TODO: 
- Better image size for Django Polls image : see distroless solution  
- Define the good way to add generic ENTRYPOINT/CMD command on Django polls and jmeter  
- Write a docker-compose and use Kompose for getting a first version of K8s yaml manifests  
- Customize all pods definitions: Django polls, jmeter by adding side-car containers or init-container as helpers or logging purpose     
- See Skaffold developpment on multi-projects     
- More ansible scripts for setting up the environment K8s from scratch  
- More monitoring, add Django polls app in prometheus monitoring, add application monitoring     
- More Logging for Grafana Loki   
- Deployment, set up, Replica to 4, Strategy 25%, type of deployment: Recreate, Rolling Update, Canary, A/B, Green/Blue  
- Helm, Create a Helm chart of Django Polls App
- See CI/CD solution using Jenkins, and Argo CD    
