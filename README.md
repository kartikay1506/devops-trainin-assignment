# End to End Automated Development to Deployment Setup
## About ##
The objective of this assignment it to create an end to end automated pipeline. Whenever the developer makes the changes to a branch (in our case dev) or master branch, the code is commited then pushed to github, pulled from github by testing team, verify changes, and approve the code for deployment on production server. This is a tedious and error prone task. To automate the complete flow of code from development to testing to deployment on production server we have setup the following infrastructure using **Jenkins**, **Docker** and **Git**, **Github**.

_**Assumptions**_
* Jenkins and Docker are already installed in RHEL8 OS
* Jenkins is added in the sudoers list
* User has a github account setup and git bash installed on the development system

**PART-1 Setting up local workspace and remote repository (github)**

* Create a workspace in any desired location.

* Create a file index.html and enter some code in that file.

![](https://github.com/kartikay1506/devops-trainin-assignment/blob/master/images/2020-05-06%20(17).png)

* Open git bash in the current workspace. WE have our files here.

![](https://github.com/kartikay1506/devops-trainin-assignment/blob/master/images/2020-05-06%20(16).png)

* Create a branch dev and switch to that branch as following:

![](https://github.com/kartikay1506/devops-trainin-assignment/blob/master/images/2020-05-06%20(19).png)

* Edit some code in this branch.

* Create file in the folder .git/hooks by the name of &quot;post-commit&quot; which is known as the post commit hook and add the following lines of code:

```bash
#! /bin/bash
git push
```

* Add the remote repository as origin:
```bash
git remote add origin https://github.com/kartikay1506/devops-training
```

*  Add all the files in dev branch to staging area and commit using the following code:
```bash
git add \*
git commit -m "First Commit"
```
_As soon as the code is committed it will automatically be pushed to github repository by the post commit hook we created in step 6_

9. We can now see that our repository is updated and has two branches: master and dev

![](RackMultipart20200506-4-ap3eb5_html_b02023e6a81dd90a.png)

**PART -2 Setting Up Jenkins**

1. Setup Job 1 as given by following images:

Give Job Name

![](RackMultipart20200506-4-ap3eb5_html_5b5966914ceda738.png)

Add the link of the github repository created before.

![](RackMultipart20200506-4-ap3eb5_html_42fbd18885258d36.png)

Select to Build the job through remote trigger and save the auth token for later use as follow:

As soon as the developer will commit the code on master branch this job will be triggered and the code will be deployed on the production server.

![](RackMultipart20200506-4-ap3eb5_html_a2790f13d37074e9.png)

Add the following script to be executed on building the job

![](RackMultipart20200506-4-ap3eb5_html_1c8d25b58a3b7dc3.png)

Repeat the above steps for Job 2 with minor changes in the links

![](RackMultipart20200506-4-ap3eb5_html_b295745853bcd1d3.png)

Select dev as the branch to be used

![](RackMultipart20200506-4-ap3eb5_html_97419a8cbf99988d.png)

Configure the job to be triggered remotely for building and save the auth token for later use.

As soon as the developer will commit the code on dev branch this job will be triggered and the code will be deployed on the testing server.

![](RackMultipart20200506-4-ap3eb5_html_fb2e557bbc9d1353.png)

Add the following script to executed while building the job

![](RackMultipart20200506-4-ap3eb5_html_16842bc60a37026f.png)

Configure the final test job

![](RackMultipart20200506-4-ap3eb5_html_edaf12f14576fbab.png)

Add the repository link

![](RackMultipart20200506-4-ap3eb5_html_86a99ae8ebdd033c.png)

Add remote build trigger

![](RackMultipart20200506-4-ap3eb5_html_ca75a3d71aaad04e.png)

Now add post build action that will merge the branch with master branch when the test team will trigger this job upon verifying the changes in the dev branch and as soon as this job is build successfully, the dev branch will be merged with master branch and job 1 will be triggered for deploying the verified code on the production server.

![](RackMultipart20200506-4-ap3eb5_html_9a93786d9c2ed14.png)
