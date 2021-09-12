# kubernetes-prometheus
## Install prometheus 
Prometheus monitoring on Kubernetes cluster.

Hit the following commands: 
```shell script
k create -f clusterRole.yaml  # cluster role
k create -f config-map.yaml  # config map and settings
k create -f prometheus-deployment.yaml  # deployment
k expose -n monitoring deployment/prometheus-deployment # quick install for KinD
k get svc -A  # check 
k -n monitoring port-forward service/prometheus-deployment  3750:9090
 # Check with your browser 
```
