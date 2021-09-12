# Jmeter for k8s 

## install image in dockerhub ( or in your private hub) 
```shell
docker build -t systemdevformations/jmeter-k8s .  # docker account prefix 
docker push systemdevformations/jmeter-k8s   # save in docker hub
```

##  start a deployment 
```shell
k create deployment jmeter --image=systemdevformations/jmeter-k8s
# get yaml file 
k get deployment jmeter -o yaml | kubectl-neat > jmeter-deployment.yaml
#add lines
command: [ "/usr/bin/tail" ]  # added
args: [ "-f", "/dev/null" ]   # added
k delete deployment jmeter
k create -f jmeter-deployment.yaml 
# get cluster IP address
k get pod -o wide -w # pod web IP 
k exec -it jmeter-xxxx -- /bin/ash  # get inside jmeter pod 
jmeter # Check 
# install test file 
jmeter -JIP=10.96.23.81 -Jjmeter.save.saveservice.output_format=xml -Jjmeter.save.saveservice.response_data.on_error=true -n -t  polls_test_plan.jmx  -l testresult.jlt
```

## TODO 
should be more generic 
