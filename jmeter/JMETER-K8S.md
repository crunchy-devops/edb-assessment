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

## RESULTS
-JIP=10.244.3.7 -JUSER=10 -JRAMP_UP=180 -JLOOP=5
Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
summary +    241 in 00:00:37 =    6.6/s Avg:    23 Min:     4 Max:    58 Err:     0 (0.00%) Active: 1 Started: 3 Finished: 2  
summary +    200 in 00:00:35 =    5.7/s Avg:    22 Min:     4 Max:    57 Err:     0 (0.00%) Active: 1 Started: 5 Finished: 4  
summary =    441 in 00:01:12 =    6.1/s Avg:    23 Min:     4 Max:    58 Err:     0 (0.00%)  
summary +    347 in 00:00:55 =    6.3/s Avg:    23 Min:     4 Max:    68 Err:     0 (0.00%) Active: 1 Started: 8 Finished: 7  
summary =    788 in 00:02:07 =    6.2/s Avg:    23 Min:     4 Max:    68 Err:     0 (0.00%)  
summary +    203 in 00:00:35 =    5.8/s Avg:    73 Min:     4 Max:   896 Err:     0 (0.00%) Active: 1 Started: 10 Finished: 9  
summary =    991 in 00:02:42 =    6.1/s Avg:    33 Min:     4 Max:   896 Err:     0 (0.00%)  
summary +    109 in 00:00:04 =   24.8/s Avg:    22 Min:     4 Max:    66 Err:     0 (0.00%) Active: 0 Started: 10 Finished: 10  
summary =   1100 in 00:02:46 =    6.6/s Avg:    32 Min:     4 Max:   896 Err:     0 (0.00%)  
