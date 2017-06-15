Here are some commonly asked questions.

### I try to login and I see my username is not approved.
Click on the `Register Now` link in the landing page, and sign-in using your `@redhat.com` account.


### I am an approved user, but I'm unable to get past the landing page.
< to be filled in >


### The username in Keycloak is different than what I see on OpenShift.io
This is a bug which is being fixed in https://github.com/almighty/almighty-core/pull/1367 
After this PR is merged, re-login to OpenShift.io to be able to see your updated username.



### Where are my applications running ?
Currently, the tenant URL is https://console.starter-us-east-2.openshift.com 



### My username on OpenShift.io is different than the name of the project created in my OpenShift Online account
The reason is unknown as of now, however the debugging session was successful in resolving this for `chmouel`
This will cause issues with creating your QuickStart projects and pipelines.
( need to fill in details ) 



### How do I clean-up all apps and builds in my OpenShift Online 

In OpenShift Online, go to the Help question mark (?) in the upper right. 
 1. Select "Command Line Tools"
 2. Use the copy icon to copy the first command to get your token
 3. Paste that into a terminal
 4. Use the commands listed below, substituting `${ID}` for your account login ID

```
oc delete bc --all -n ${ID}
oc delete build --all -n ${ID}
oc delete all --all -n ${ID}-test
oc delete all --all -n ${ID}-stage
oc delete all --all -n ${ID}-run
```


### I already have a project I had created in my OpenShift Online account.

Assuming your old project is `pke-test1` and your OpenShift.io username is `pkettman`, this might cause your pipelines to not show up since there is no `pkettman` namespace.

Clean-up your existing project by running the following:

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


### My Pipeline build fails with out-of-memory exceptions

``` 
[Pipeline] End of Pipeline
java.lang.OutOfMemoryError: Metaspace
Finished: FAILURE
```

The solution is same as the one above, if you are using the OpenShift CLI, you could use these command alternatively to restart the Jenkins pod

```
oc scale dc/jenkins --replicas=0 -n burrsutter-jenkins
oc scale dc/jenkins --replicas=1 -n burrsutter-jenkins
```



https://github.com/openshiftio/openshift.io/issues/206




### How do I enable or disable integration tests in my pipelines?

You can edit your `Jenkinsfile` to comment out the `mavenIntegrationTest` section on a per app basis.

If you are doing demos you may want to disable integration tests to speed up demos ;) To do that you can globally configure pipelines (once the new Tenant is released) to disable integration tests via updating your `fabric8-pipelines` `ConfigMap`.

Type:
```
oc edit cm fabric8-pipelines
```
Then add the following to the `data:` section of the `ConfigMap` to disable the `CD` and/or `CI` integration tests
```yaml
data:
  disable-itests-cd: 'true'
  disable-itests-ci: 'true'
```

**NOTE** that there is [an issue with RHOAR boosters](https://github.com/openshiftio/booster-common/issues/8) so we've temporarily disabled all integration tests anyway!

### My Pipelines are not being properly filtered by my Space?

When you switch between spaces the Pipelines tab should filter by those apps created in the space.

If you created an App a while back (say before June 2017) then the created Pipeline (which is a BuildConfig inside OpenShift) will not have the space label associated so that per space filtering doesn't work.

To work around this from the openshift command line type:

```
oc edit bc foo
```
then ensure that the `metadata.labels` has a value of `space: cheese` like this:

```yaml
metadata:
  name: foo
  labels:
     space: cheese
...
```
that will then ensure that the `foo` pipeline is filtered to only show on the `cheese` space!

### How do I update my tenant ?

Your own jenkins,che,etc is running in your OpenShift Online account in the https://console.starter-us-east-2.openshift.com cluster. The Deployment Configs have to be updated every few days. To do so, 
* Login to OpenShift.io , if you face issues while trying to login, please see (  add link )
* Visit your profile by clicking on the top-right corner.
* Click "Update profile"
* Scroll down to the bottom of the page and click "Update tenant". This doesn't provide any feedback as of now, but you could check for a `/api/user 200 OK` response in the browser console.


### How do I confirm that my tenant is updated ?

To confirm you need to be checking which build version of Jenkins/Che you are running, and compare it with the intended version.

To find out which version of the Jenkins 2 build you are running,
use the `oc` CLI , and try running `oc export dc jenkins -n oc {ID}-jenkins | grep "version:"`

If the above doesn't work, use the UI:
* Login to OpenShift Online https://console.starter-us-east-2.openshift.com
* Select project {ID}-jenkins
* Navigate to ``Applications` -> `Deployments` -> `jenkins`.
* Open the latest 'active' deployment from the list.
* On the `details` tab, there should a `version=3.x.x`


### Which version should my Jenkins be at at ?

To find out the intended version, visit http://central.maven.org/maven2/io/fabric8/online/packages/fabric8-online-jenkins/1.0.167/fabric8-online-jenkins-1.0.167-openshift.yml
and look for the section:


```
provider: fabric8
      project: jenkins-openshift
      version: 3.0.37
```

Re-construct the above URL by replacing **1.0.167** with the value in the file "TEAM_VERSION" in https://github.com/fabric8io/fabric8-init-tenant/commit/COMMIT_HASH where COMMIT_HASH is the hash in https://github.com/openshiftio/saas/blob/master/dsaas-services/f8-tenant.yaml#L2


### Which version should my Che be at ?

To find out the intended version, visit http://central.maven.org/maven2/io/fabric8/online/packages/fabric8-online-jenkins/1.0.167/fabric8-online-che-1.0.167-openshift.yml and look for the che version inside it.


Re-construct the above URL by replacing **1.0.167** with the value in the file "TEAM_VERSION" in https://github.com/fabric8io/fabric8-init-tenant/commit/COMMIT_HASH where COMMIT_HASH is the hash in https://github.com/openshiftio/saas/blob/master/dsaas-services/f8-tenant.yaml#L2



### My tenant update does not work
https://github.com/fabric8io/fabric8-init-tenant/issues/74


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



### Whenever my build completes it starts another one

This could have happened if lots of builds were triggered and never got provisioned due to quotas and there's lots of Build resources left over. 

Delete all the build resources by doing a `oc delete build --all`




### Configured service account doesn't have access

`Unauthorized! Configured service account doesn't have access. Service account may have been revoked. Unauthorized`

* Go to https://sso.openshift.io/auth/realms/fabric8/account/identity
* Remove and add your Openshift-v3 account.
* Remove and add your Github account.

