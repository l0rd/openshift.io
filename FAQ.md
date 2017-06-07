Here are some commonly asked questions.

### Jenkins is not responding, says "Application is not available"

When I goto https://jenkins-<username>-jenkins.8a09.starter-us-east-2.openshiftapps.com I just get Application Not Available. 

### [How to test a local change in planner end to end?]

1/ Point to local env in fabric8-ui: 
```
export FABRIC8_WIT_API_URL="http://localhost:8080/api/"
export FABRIC8_REALM="fabric8-test"
```

2/ In almighty-go, go to your PR branch
```
cd ~/workspace/go/src/github.com/almighty/almighty-core
git checkout MY_BRANCH
```
(optionally) flush the DB
```
make dev
```

3/ in fabric8-ui
```
cd ~/workspace/fabric8-ui
npm i
```
Note: both fabric8-ui and fabric8-planner should be placed in sibling folders

4/ in fabric8-planner
```
cd ~/workspace/fabric8-planner
npm i
scripts/run-planner.sh -r
```
the script will do for you:
* create a folder `dist-watcher` 
* link it to fabric8-ui)
* go to fabric8-ui and  run `npm start`

### How to draw a diagram of the DB
* `brew install graphviz`
* `brew install postgres`
* Download https://jdbc.postgresql.org/download/postgresql-42.1.1.jar
* Download https://sourceforge.net/projects/schemaspy/files/schemaspy/SchemaSpy%205.0.0/schemaSpy_5.0.0.jar/download?use_mirror=kent&r=https%3A%2F%2Fsourceforge.net%2Fprojects%2Fschemaspy%2Ffiles%2Fschemaspy%2F&use_mirror=kent 
* Copy both jar in current directory
```
 java -jar schemaSpy_5.0.0.jar -t pgsql -db postgres -host 127.0.0.1 -u postgres -p mysecretpassword -o ./schemaspy -dp postgresql-42.1.1.jar -s public -noads
```

### How to work on dependent module from fabric8-ui 
* use npm link
```
cd ~/projects/dependency 
npm link                 
cd ~/projects/myproject
npm link dependency             # links to your local dependency
```
* to unlink
```
npm unlink dependency
npm install
```

* find if still some linked module
```
find node_modules -maxdepth 1 -type l -ls
```