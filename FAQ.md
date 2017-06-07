Here are some commonly asked questions.

### Jenkins is not responding, says "Application is not available"

When I goto https://jenkins-<username>-jenkins.8a09.starter-us-east-2.openshiftapps.com I just get Application Not Available. 

### How do I update my tenant ?

### My tenant update does not work even

### My QuickStart project fails to initialize

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