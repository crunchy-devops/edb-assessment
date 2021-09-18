# Monitoring 

## Install prometheus
Go to ~/edb-assessment/kind/prometheus/README.me

## expose cluster-example pod 
```shell
k expose pod/cluster-example-1
k expose pod/cluster-example-2
k expose pod/cluster-example-3
k get svc # check
```

## Change prometheus config map 
```shell

```



## Get Postgresql pod instance metrics
```shell
kubectl port-forward pod/cluster-example-1 3500:9187
# check in your browser
http://localhost:3500/metrics 
```

## Set prometheus node exporter 

