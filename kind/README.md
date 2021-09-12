# using KinD

## Install KinD for Kubernetes
```shell
cd   # back to the home directory
docker ps   # check the docker containers
docker --version  # check docker version 
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64 # get latest version of kind
chmod +x ./kind  # set kind as an executable
sudo mv ./kind /usr/local/bin/kind  # move to a system directory
kind version  # check the version of kind 
cd ~/edb-assessment/kind # change to the project
kind create cluster --name=pg  --config kind-config.yml # install kind kubernetes cluster # Notice: should be version 1.21
# work in a container to keep my vm/desktop clean 
docker run -it --rm --name work -v ${HOME}:/root/ -v ${PWD}:/work -w /work --net host alpine sh
```

## Install Cloud Native Postgresql in KinD
```shell
#in the alpine container
apk add --no-cache --virtual .build-deps bash gcc musl-dev openssl go curl vim # Install useful packages
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" # Download a version of kubectl
install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl # install kubectl
kubectl version --client # Check 
alias k='kubectl'  # useful alias
k get nodes  # Check 
kubectl apply -f https://get.enterprisedb.io/cnp/postgresql-operator-1.7.1.yaml # Run the operator
kubectl get deploy -n postgresql-operator-system postgresql-operator-controller-manager # check 
k get pod -A # check 
k apply -f cluster-example.yaml
k get pod -w # wait for a while
Ctrl-C
k get pod  # Check
k get svc  # Check 
k get cluster cluster-example -o yaml # Check the version of postgresql # it's 13.3
```

## Get a grip on the cluster-example 
```shell
k get Secret cluster-example-app -o yaml # find the secret
# select username
echo "YXBw" | base64 -d # get the username
echo "YTHxxxx...xxx" | base64 -d # get the password
kubectl port-forward service/cluster-example-rw 3000:5432 # forward port 5432 to 3000 localhost
# check the connectivity using goland jetbrains
# connection string  user: app, password , db: postgres, port 3000
# Check  
```

## Customize django app project
Open a new terminal
Change setting.py accordingly to your postgresql operator configuration  
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'postgres',
        'USER': 'app',
        'PASSWORD': 'a8GV8XF6qk7TydJqaUqGse0PyjikzgWhwHqA6tVKw5S6PnOW55OQHcnbsfZgztPb',
        'HOST': 'cluster-example-rw',
        'PORT': '5432',
    }
}
```
## Build dedicated image for k8s
```shell 
docker login 
# keyed your credentials
docker build -t systemdevformations/poll-k8s . 
docker push systemdevformations/poll-k8s
```
## Quick deployment
back to your alpine container 
```shell
k create deployment web --image=systemdevformations/poll-k8s --port=8000
# Check but it's failed 
# pull image is ok 
k logs -f  web-69675d44d4-d5f5t 
k get events
```
## install kubectl-neat 
See https://github.com/itaysk/kubectl-neat
```shell
mkdir tools
cd tools
wget https://github.com/itaysk/kubectl-neat/archive/v2.0.3.tar.gz
tar -zxvf v2.0.3.tar.gz 
cd kubectl-neat-2.0.3/
apk add build-base
make
cp dist/kubectl-neat_linux_amd64 /usr/local/bin/kubectl-neat
k get svc web -o yaml | kubectl-neat > web-service.yaml
```

## added a command in django-deployment.yaml 
```yaml
      - image: systemdevformations/poll-k8s
        command: [ "/usr/bin/tail" ]  # added
        args: [ "-f", "/dev/null" ]   # added
        imagePullPolicy: Always
        name: poll-k8s
```
## Start the deployment again  
```shell
k delete deploy web 
k create -f django-deployment.yaml
k exec -it web-xxxx -- /bin/bash
root@web-xxxx:/app python3 manage.py migrate
root@web-xxxx:/app python3 manage.py createsuperuser
# in a second terminal 
docker exec -it work /bin/ash
kubectl port-forward service/cluster-example-rw 3000:5432 
# use jetbrains to check,  should have 12 tables in postgres database 
```
## Load some data
In goland, use the database tab on right-hand side.   
Set a postgresql database connection  
Right-click on the table polls_choice and select Import Data from  File  
Select the file to load in the directory data postgres_public_polls_choice.csv
Do the same for the table polls_question

## Create django poll service 
```shell
k expose deployment/web # command line for creating a service for django polls
# get yaml file 
k get svc web -o yaml | kubectl-neat > web-service.yaml # convert to yaml
kubectl port-forward service/web 3500:8000  # start the port-forwarding
# check in a browser 
# http://localhost:3500/polls
```

## Install prometheus
See README.md in prometheus directory    
**Not enough time to carry on investigating Prometheus with Cloud Native Postgresql**



