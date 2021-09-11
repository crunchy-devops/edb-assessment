# using KinD

## push django polls image to dockerhub registry
```shell
cd   # back to the home directory
docker login -u xxxx -p xxxx
docker tag poll systemdevformations/poll
docker push systemdevformations/poll

## Install KinD for Kubernetes
```shell
cd   # back to the home directory
docker ps   # check the docker containers
docker --version  # check docker version 
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64 # get latest version of kind
chmod +x ./kind  # set kind as an executable
sudo mv ./kind /usr/local/bin/kind  # move to a system directory
kind version  # check the version of kind 
kind create cluster --name=pg  # install kind kubernetes cluster # Notice: should be version 1.21
cd ~/edb-assessment # change to the project
# work in a container to keep my vm clean 
docker run -it --rm --name work -v ${HOME}:/root/ -v ${PWD}:/work -w /work --net host alpine sh
```

## Install Cloud Native Postgresql in Kind 
```shell
#in the apline container
apk add --no-cache --virtual .build-deps bash gcc musl-dev openssl go curl vim # Install useful packages
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" # Download a version of kubectl
install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl # install kubectl
kubectl version --client # Check 
alias k='kubectl'  # useful alias
k get nodes  # Check 
kubectl apply -f https://get.enterprisedb.io/cnp/postgresql-operator-1.7.1.yaml # Run the operator
kubectl get deploy -n postgresql-operator-system postgresql-operator-controller-manager # check 
k get pod -A # check 
cd kind # modve to the working directory
k apply -f cluster-example.yaml
k get pod -w # wait for a while
Ctrl-C
k get pod  # Check
k get svc  # Check 
```

