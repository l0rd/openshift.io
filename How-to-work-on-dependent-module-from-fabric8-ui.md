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