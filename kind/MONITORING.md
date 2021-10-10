# Monitoring 

## Install prometheus
Go to ~/edb-assessment/kind/prometheus/README.md

## expose cluster-example pod 
```shell
k expose pod/cluster-example-1
k expose pod/cluster-example-2
k expose pod/cluster-example-3
k get svc # check
```

## Check how the metrics are scraped
```shell
kubectl port-forward svc/cluster-example-1 3650:9187
# check in your browser
http://localhost:3650/metrics
# Scrape is done cnp_xx names
```

## Change prometheus.yml config-map in prometheus/config-map.yaml file
```yaml
      - job_name: 'kube-state-metrics'
        static_configs:
          - targets: ['kube-state-metrics.kube-system.svc.cluster.local:8080']
          # TODO: I have to find a regex for being dynamic on cluster-example name 
          - targets: ['cluster-example-1.default.svc.cluster.local:9187']  # Added
          - targets: ['cluster-example-2.default.svc.cluster.local:9187']  # Added
          - targets: ['cluster-example-3.default.svc.cluster.local:9187']  # Added
```
## Restart prometheus 
```shell
cd promotheus
k apply -f config-map.yaml 
k delete -f prometheus-deployment.yaml 
k create -f prometheus-deployment.yaml 
k -n monitoring port-forward service/prometheus-deployment  3750:9090
```

## Install Grafana
```shell
cd prometheus
k apply -f grafana.yaml 
k -n monitoring port-forward service/grafana  3900:3000
```
## Configure Grafana 
Go to Configuration / Data Source     
Select prometheus   
Enter the URL http://localhost:3900  
select Browser   
Save   
Go to DashBoard / Manage 