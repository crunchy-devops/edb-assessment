# Jmeter
Testing performance load for Django app

## Start a jmeter container 
for doing a load test against django/postgresql POC 
```shell
cd ~/edb-assessment/jmeter  # go to the relevant directory
docker build -t jmeter .   # build jmeter docker image
docker images # check  
```

## Tests
```shell
docker run -d --name test -v ${PWD}:/test jmeter tail -f /dev/null # Start jmeter container

## Start locally the jmeter gui 
### Recording a test plan for your Django Poll website
Install Firefox and setup the add-on named FoxyProxy  
In foxyproxy added a proxy  
Enter a title Jmeter    
Proxy Type is HTTP  
Proxy IP is localhost    
Port is 8888  
no username or password

![FoxyProxy](screenshots/foxyproxy.png)

### Create Test plan
If firefox switch to jmeter proxy
![FoxyProxy](screenshots/Test_recorder.png)
Start with your mouse over test plan and right click on it.    
1- menu  Add -> Thread -> Thread Group  
2- Righ click on Test plan -> Add -> Non-test-elements -> HTTPS Test Script Recorder
In target controller select Test Plan ->Thread Group
Grouping : Put each group in a new controller
Click on tab Request Filtering  
Click Add in URL Patterns to Exclude   
Copy and paste these excluded files
```shell script
   .*\.(txt|bmp|css|js|gif|ico|jpe?g|png|swf|woff|woff2|ttf|txt-).*
``` 
click on start in Https test script recorder the green start button  
Ok for the jmeter certificat
Open a tab an copy paste your Django Poll URL
```shell script
     http://<your ip address>:8000/polls  
```
Check if jmeter is recording all your actions by clicking on Thread Group   
Click on a question in firefox,   
and select an answer 
click  vote again 
do again randowly the previous actions
Stop recording in the small jmeter windows  
Click file, select Save Test Plan As    
Save your test plan to your git repo  as polls_test_plan.jmx

### Set JMeter variables
Add HTTP Requests Defaults variables  
Remove all IP and port from other HTTP Request action  
Move over Thread group -> right click -> Add -> Listener -> View Results Tree  

## CSRF token and generic values  
See Test Plan for all generic values     
See Regular Expression extractor to get CSRF token, it's copy as a value in the following POST screen  
Don't miss adding an HTTP cookie manager item at the beginning of test plan to avoid any issue with the CSRF.

## CLI tests command on local host
```shell
docker exec -it test jmeter -JIP=192.168.1.85 -Jjmeter.save.saveservice.output_format=xml \
-Jjmeter.save.saveservice.response_data.on_error=true -n -t /test/tests/polls_test_plan.jmx \
-l testresult.jlt
```

## Some Tests using jmeter container from Ansible
```shell
cd ~/edb-assessment/jmeter  # go to the relevant directory
ansible-playbook -i ../inventory install-jmeter.yml 
So you can change the IP address, port number and other parameter in jmeter command 
```

# Some performance tests
-JIP=172.17.0.3 -JUSER=10 -JRAMP_UP=180 -JLOOP=5
Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
summary +    111 in 00:00:18 =    6.2/s Avg:    21 Min:     3 Max:    67 Err:     0 (0.00%) Active: 1 Started: 2 Finished: 1  
summary +    406 in 00:00:57 =    7.1/s Avg:    21 Min:     3 Max:    56 Err:     0 (0.00%) Active: 1 Started: 5 Finished: 4  
summary =    517 in 00:01:15 =    6.9/s Avg:    21 Min:     3 Max:    67 Err:     0 (0.00%)  
summary +    144 in 00:00:33 =    4.3/s Avg:    22 Min:     4 Max:    64 Err:     0 (0.00%) Active: 1 Started: 7 Finished: 6  
summary =    661 in 00:01:48 =    6.1/s Avg:    21 Min:     3 Max:    67 Err:     0 (0.00%)  
summary +    156 in 00:00:27 =    5.7/s Avg:    72 Min:     3 Max:   957 Err:     0 (0.00%) Active: 1 Started: 8 Finished: 7  
summary =    817 in 00:02:15 =    6.0/s Avg:    31 Min:     3 Max:   957 Err:     0 (0.00%)  
summary +    250 in 00:00:30 =    8.5/s Avg:    71 Min:     3 Max:  1304 Err:     0 (0.00%) Active: 1 Started: 10 Finished: 9  
summary =   1067 in 00:02:45 =    6.5/s Avg:    40 Min:     3 Max:  1304 Err:     0 (0.00%)  
summary +     33 in 00:00:01 =   26.4/s Avg:    21 Min:     3 Max:    57 Err:     0 (0.00%) Active: 0 Started: 10 Finished: 10  
summary =   1100 in 00:02:46 =    6.6/s Avg:    40 Min:     3 Max:  1304 Err:     0 (0.00%)  