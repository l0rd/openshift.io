Here are some commonly asked questions.

### Jenkins is not responding, says "Application is not available"

When I goto https://jenkins-<username>-jenkins.8a09.starter-us-east-2.openshiftapps.com I just get Application Not Available. 

### [[How to test a local change in planner end to end?]]


### [[How to draw a diagram of the DB]]

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