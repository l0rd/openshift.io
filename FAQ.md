Here are some commonly asked questions.

### Where are my applications running ?
Currently, the tenant URL is https://console.starter-us-east-2.openshift.com 

### Jenkins is not responding, says "Application is not available" 

When I goto https://jenkins-username-jenkins.8a09.starter-us-east-2.openshiftapps.com I just get Application Not Available. 

Workaround : 
* Login to https://console.starter-us-east-2.openshift.com
* Navigate to https://console.starter-us-east-2.openshift.com/console/project/your-username-jenkins/overview by clicking on the *-jenkins project.
* Scale the "jenkins" service to 0 pods and then to 1. 

This might take upto 5 minutes to take effect.

### How do I update my tenant ?

Your own jenkins,che,etc is running in your OpenShift Online account in the https://console.starter-us-east-2.openshift.com cluster. The Deployment Configs have to be updated every few days. To do so, 
* Login to Openshift.io , if you face issues while trying to login, please see < add link>
* Visit your profile by clicking on the top-right corner.
* Click "Update profile"
* Scroll down to the bottom of the page and click "Update tenant". This doesn't provide any feedback as of now, but you could check for a `/api/user 200 OK` response in the browser console.



### My tenant update does not work

### My QuickStart project fails to initialise

This happens when the the Openshift-v3 or Github expires.  

* Go to https://sso.openshift.io/auth/realms/fabric8/account/identity
* Remove and add your Openshift-v3 account.
* Remove and add your Github account.

### I add a new QuickStart project but the build doesn't start on the Pipelines page.

This happens because of a bug in the jenkins-sync plugin which restarts all old builds. Since, we have a restriction of only allowing 1 build at a time, the newly created project's build pipeline falls low into the build queue. To fix this, abort all the queued up builds except the one which was created 'just now'.

* On the Pipelines UI, click on the Jenkins URL ( this part needs better instructions )
* In the Jenkins UI, locate the queue visualisation on the left-hand side of the page. 
* Click on the red abort button for each build.

### Configured service account doesn't have access

`Unauthorized! Configured service account doesn't have access. Service account may have been revoked. Unauthorized`

* Go to https://sso.openshift.io/auth/realms/fabric8/account/identity
* Remove and add your Openshift-v3 account.
* Remove and add your Github account.

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