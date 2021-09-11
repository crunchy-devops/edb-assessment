# Jmeter
Testing performance load for Django app

## Start a jmeter container 
for doing a load test against django/postgresql POC 
```shell
cd ~/edb-assessment/jmeter  # go to the relevant directory
docker build -t jmeter .   # build jmeter docker image
docker images # check  
```
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
Start with your mouse over test plan and right click on it.    
menu  Add -> Thread -> Thread Group  
Righ click on Test plan -> Add -> Non-test-elements -> HTTPS Test Script Recorder
![FoxyProxy](screenshots/Test_recorder.png)  
In target controller select Test Plan ->Thread Group
Grouping : Put each group in a new controller
Click on tab Request Filtering  
Click Add in URL Patterns to Exclude   
Copy and paste these excluded files
```shell script
   .*\.(txt|bmp|css|js|gif|ico|jpe?g|png|swf|woff|woff2|ttf).*
```
switch firefox browser to user jmeter port  
click on start in Https test script recorder   
Open a tab an copy paste your petclinic URL
```shell script
     http://<your ip address>:8000/poll  
```
Accept the temporary certificates created by JMeter   
Check if jmeter is recording all your actions by clicking on Thread Group   
Click on a question,   
and select an answer 
click   
do against randowly the previous actions
Stop recording in the small jmeter windows  
Click file, select Save Test Plan As    
Save your test plan to your git repo  as poll_test_plan.jmx

### Set JMeter variables
Add HTTP Requests Defaults variables
Remove all IP and port from other HTTP Request action
Move over Thread group -> right click -> Add -> Listener -> View Results Tree 