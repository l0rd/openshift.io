### How to test a local change in planner end to end?

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