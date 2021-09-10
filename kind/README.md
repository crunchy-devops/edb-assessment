# KinD

## Install KinD for Kubernetes
``` shell
docker ps   # check the docker containers
docker --version  # check docker version 
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64 # get latest version of kind
chmod +x ./kind  # set kind as an executable
sudo mv ./kind /usr/local/bin/kind  # move to a system directory
kind version  # check the version of kind 
kind create cluster --name=pg  # install kind kubernetes cluster # Notice: should be version 1.21
docker run -it --rm --name work -v ${HOME}:/root/ -v ${PWD}:/work -w /work --net host alpine sh
```

## Install Cloud Native Postgresql in Kind 
```shell
apk add --no-cache --virtual .build-deps bash gcc musl-dev openssl go curl vim # Install usefull packages
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" # Download a version of kubectl
install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl # install kubectl
kubectl version --client # Check 
alias k='kubectl'  # useful alias
k get nodes  # Check 
kubectl apply -f https://get.enterprisedb.io/cnp/postgresql-operator-1.7.1.yaml # Run the operator
kubectl get deploy -n postgresql-operator-system postgresql-operator-controller-manager # check 
k get pod -A # check 
kubectl apply -f cluster.yaml
k get pod -w
19 k get pod
21 k get svc
```