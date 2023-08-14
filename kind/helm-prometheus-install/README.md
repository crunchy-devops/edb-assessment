#  Helm
## Install helm
```shell script
wget https://github.com/helm/helm/archive/refs/tags/v3.6.3.tar.gz
tar -zxvf v3.6.3.tar.gz
cd helm-3.6.3/
make
cd bin
sudo cp  helm /usr/local/bin/helm
helm version
```
## Install prometheus using helm 
```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm search repo prometheus-community
helm install my-release prometheus-community/kube-prometheus-stack
```

## Check 
```shell
kubectl port-forward service/prometheus-operated 3000:9090 --address='0.0.0.0'
```