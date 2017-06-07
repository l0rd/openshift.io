Here are some commonly asked questions.

### Jenkins is not responding, says "Application is not available" 

When I goto https://jenkins-<username>-jenkins.8a09.starter-us-east-2.openshiftapps.com I just get Application Not Available. 

### How do I update my tenant ?
Your own jenkins,che,etc is running in your OpenShift Online account in the https://console.starter-us-east-2.openshift.com cluster. The Deployment Configs have to be updated every few days. To do so, 
* Login to Openshift.io , if you face issues while trying to login, please see < add link>
* Visit your profile by clicking on the top-right corner.
* Click "Update profile"
* Scroll down to the bottom of the page and click "Update tenant". This doesn't provide any feedback as of now, but you could check for a `/api/user 200 OK` response in the browser console.



### My tenant update does not work

### My QuickStart project fails to initialise

### I add a new QuickStart project but the build doesn't start on the Pipelines page.

### [[How to test a local change in planner end to end?]]


### [[How to draw a diagram of the DB]]

### [[How to work on dependent module from fabric8-ui]] 

### How to reinit DB on almighty-core
```
docker rm -f almightycore_db_1 && docker-compose up -d db
make dev
```
### How to access Almighty DB
* Method1: if you have psql locally installed:
```
psql -h 127.0.0.1 -d postgres -U postgres -W 
```
* Method2: running psql within docker
```
docker exec -it almightycore_db_1 bash -c psql
```

### How to run one unit test
```
ALMIGHTY_RESOURCE_DATABASE=1 ALMIGHTY_DEVELOPER_MODE_ENABLED=true  go test github.com/almighty/almighty-core/controller -v -run TestSuiteWorkItemLink -testify.m TestCreateAndDeleteWorkItemLinkOK
```
will run all test suite starting with `TestSuiteWorkItemLink` and within those, all the tests starting with `TestCreateAndDeleteWorkItemLinkOK`

### How to re-generate design
```
rm -rf app/*.go && make generate
```