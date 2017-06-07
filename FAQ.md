Here are some commonly asked questions.

### I try to login and I see _Username_ not approved.
Click on the `Register Now` link in the landing page, and sign-in using your `@redhat.com` account.

### I am an approved user, but I'm unable to get past the landing page.


### The username in Keycloak is different than what I see on OpenShift.io
This is a bug which is being fixed in https://github.com/almighty/almighty-core/pull/1367 
After this PR is merged, re-login to OpenShift.io to be able to see your updated username.

### Where are my applications running ?
Currently, the tenant URL is https://console.starter-us-east-2.openshift.com 

### My username on OpenShift.io is different than the name of the project created in my OpenShift online account

This will cause issues with creating your QuickStart projects and pipelines.

### I already have a project I had created in my OpenShift Online account.

Assuming your old project is `pke-test1` and your OpenShift.io username is `pkettman`, this might cause your pipelines to not show up since there is no `pkettman` namespace.

1. Run the following:

Visit https://console.starter-us-east-2.openshift.com/oauth/token/request to get your `oc` auth token needed for the CLI login.
```
oc login https://console.starter-us-east-2.openshift.com  --token=TOKEN
oc delete project pke-test1
oc new-project pkettman
oc delete all --all -n pkettman-test
oc delete all --all -n pkettman-stage
oc delete all --all -n pkettman-run
```

### Jenkins is not responding, says "Application is not available" 

"When I goto `https://jenkins-username-jenkins.8a09.starter-us-east-2.openshiftapps.com` I just get Application Not Available"

This happens when your Jenkins service is struggling to load because of resource issues.

Workaround : 
* Login to https://console.starter-us-east-2.openshift.com
* Navigate to https://console.starter-us-east-2.openshift.com/console/project/your-username-jenkins/overview by clicking on the *-jenkins project.
* Scale the "jenkins" service to 0 pods and then to 1. 

This might take upto 5 minutes to take effect.

### How do I update my tenant ?

Your own jenkins,che,etc is running in your OpenShift Online account in the https://console.starter-us-east-2.openshift.com cluster. The Deployment Configs have to be updated every few days. To do so, 
* Login to OpenShift.io , if you face issues while trying to login, please see < add link>
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
make clean-generated && make generate
```