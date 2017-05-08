![](./media/image35.png)

#Lab 3 - Build and Deploy using Maven, Grunt and Jenkins

##Overview

Often the most difficult and confusing aspect of application development
is figuring out how to build a common framework for managing new
applications. Over time, development teams have started using tools like
Maven, Ant and Ivy to automate some of these functions.

In this lab, you will use GitHub to manage the source of your API
Bundle, Maven and Grunt to create administrative tasks/job on Apigee
Edge and Jenkins as the continuous integration tool to start/manage
those tasks/job.

##Objectives

In this lab you will perform all the high level activities associated
with continuous development/integration.

You will start with the development of a simple API proxy. You will
prepare that proxy for checking into a source code repository. You will
then use a continuous integration tool (Jenkins) to run maven tasks to
build and deploy Apigee API Proxies.

![](.//media/image66.png)

### Tools Used

Apigee Maven Plugin:
[*https://github.com/apigee/apigee-deploy-maven-plugin*](https://github.com/apigee/apigee-deploy-maven-plugin)

Apigee Grunt Plugin:
[*https://github.com/apigeecs/apigee-deploy-grunt-plugin*](https://github.com/apigeecs/apigee-deploy-grunt-plugin)

Jenkins:
[*https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Red+Hat+distributions*](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Red+Hat+distributions)

Machine Prerequisites
---------------------

Git client must be installed on your laptop. You download the client
from here:
[*https://git-scm.com/downloads*](https://git-scm.com/downloads)

Lab Prerequisites
-----------------

### Prerequisite 1

Sign up for a GitHub account here:
[*https://github.com/join*](https://github.com/join)

### Prerequisite 2

Lab 1 of DevJam is complete

-or-

Perform the following steps to deploy a sample proxy.

-   Download the API Bundle to your local workstation from here:
    [https://github.com/apigee/DevJam/blob/master/DevOps/Resources/hotelsapi.zip](https://github.com/apigee/DevJam/blob/devopsjam/DevOps/Resources/hotelsapi.zip)

-   Login to Apigee Edge
    [https://accounts.apigee.com/accounts/sign_in?callback=https://enterprise.apigee.com](https://accounts.apigee.com/accounts/sign_in?callback=https://enterprise.apigee.com)

-   Navigate to API Proxies menu

![](.//media/image97.png)

-   Create a new proxy by clicking on ** API Proxy**

![](.//media/image94.png)

-   Select **API Bundle** and Click **Next**

![](.//media/image19.png)

-   Select the zip file that was downloaded from **Step 1** and Click
    **Next**

![](.//media/image99.png)

**NOTE:** Edit the Proxy name to something unique like
**{your\_initial}\_hotelsapi**

-   Click on **Build**

-   Click on the API and enter the overview page

![](.//media/image61.png)

-   Click on **Develop**

![](.//media/image71.png)

-   Click on **Proxy Endpoints, default**

![](.//media/image16.png)

-   Enter a unique basepath. For ex: **/{your\_inital}/hotelsapi**

![](.//media/image96.png)

-   Save your changes

![](.//media/image113.png)

-   Deploy the API to the **test** environment

![](.//media/image88.png)

-   You can test the API by running this cURL command:

![](.//media/image79.png)

Command:
```
curl -i http://{org-name}-{env}.apigee.net/hotelsapi
```

Now, you are all set to perform the rest of the lab.

Lab Steps
---------

### Part 1 Download and prepare the API Bundle for Maven 

1)  Download the API Bundle to your local workstation

> ![](.//media/image101.png){width="3.25in" height="3.32199365704287in"}

1)  Extract the zip file (ex: hotelsapi-2\_rev1\_2016\_03\_30.zip) to
    a folder. You will see a folder called ‘apiproxy’

> ![](.//media/image105.png)

1)  Create a folder structure as follows:

> ![](.//media/image103.png)

a.  Root folder: This is your repository name **“opsjam-test”**

b.  Under the root folder create a folder called **“src”**. This is
    where the API bundle (proxy configuration) will reside.

c.  Under the **“src”** folder, create a folder called **“gateway”**

d.  Under the **“gateway”** folder, create a folder with the name of
    your API **“hotelsapi”**

e.  Move the **“apiproxy”** folder under **“hotelsapi”**

f.  Add the
    [*shared-pom.xml*](https://github.com/apigee/DevJam/blob/master/DevOps/Resources/shared-pom.xml)
    file under the **gateway** folder

g.  Add the
    [*pom.xm*](https://github.com/apigee/DevJam/blob/master/DevOps/Resources/pom.xml)
    file under the **hotelsapi** folder

h.  Add the [*config.json.txt*](https://github.com/apigee/DevJam/blob/master/DevOps/Resources/config.json.txt) file under the **apiproxy** folder

**NOTE**: We have named the file **config.json.txt** instead of **config.json**. Leave the file name as-is for now.

#### Understanding shared-pom.xml

This file contains Apigee Edge organization and environment properties.
Each environment is setup as a profile:
```
<profile>
    <id>test</id>
    <properties>
        <options>validate</options>
        <apigee.profile>test</apigee.profile>
        <apigee.hosturl>https://api.enterprise.apigee.com</apigee.hosturl>
        <apigee.apiversion>v1</apigee.apiversion>
        <apigee.org>\${org}</apigee.org>
        <apigee.username>\${username}</apigee.username>
        <apigee.password>\${password}</apigee.password>
    </properties>
</profile>
```

#### Understanding pom.xml

This file contains reference to the share-pom file and API specific
details.
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http:/git/maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>parent-pom</artifactId>
        <groupId>apigee</groupId>
        <version>1.0</version>
        <relativePath>../shared-pom.xml</relativePath>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <groupId>apigee</groupId>
    <artifactId>hotelsapi</artifactId>
    <version>1.0</version>
    <name>hotelsapi</name>
    <packaging>pom</packaging>
</project>
```
### Part 2 - Setup GitHub

1)  Create a repository on GitHub (ensure the name matches with the root
    folder above)

    a.  Click on **+ New Repository**

> ![](.//media/image107.png)

a.  Enter a new repository name

> ![](.//media/image106.png)

a.  If the repository is successfully created, you will see this

> ![](.//media/image109.png)

a.  Grab the link to the Git Repository (HTTP)

> ![](.//media/image108.png)

1)  Initialize the local Git repository

> ![](.//media/image111.png)

Run the following command under the **opsjam-test** folder.

Command:
```
git init
```

1)  Set remote repository from the URL obtained on **Step 4d**

> ![](.//media/image110.png)

> Command:
```
git remote add origin https://github.com/{repo-name}/opsjam-test.git
git remote -v
```

1)  Add all local files to the local Git repository

> ![](.//media/image112.png)

Command:
```
git add --all
```

1)  Commit all changes to the local Git repository

> ![](.//media/image114.png)

Command:
```
git commit -m 'first version of the api'
```

1)  Push changes to the remote Git repository

> ![](.//media/image115.png)

Command:
```
git push origin master
```

### Part 3 – Build and deploy the API Bundle via Maven and Jenkins

1)  Login to Jenkins here:
    [*http://54.225.22.81:8080/*](http://54.225.22.81:8080/)[](http://54.87.93.40:8080/)

2)  Find the “maven-deploy-bundle” job

> ![](.//media/image118.png)

1)  Schedule a new job by clicking on

> ![](.//media/image119.png)

1)  Enter the details to deploy your proxy

> ![](.//media/image120.png){width="5.64in"
> height="3.6718755468066493in"}
>
> NOTE: Please see below for other options

1)  Click on “Build”

> <span id="h.gjdgxs" class="anchor"></span>

1)  Click the build id associated with your task

> ![](.//media/image121.png)

1)  Then select console output to view the build process

> ![](.//media/image122.png)

1)  View the output and make sure the build was successful

> ![](.//media/image123.png)

1)  Confirm the deployed proxy in the Enterprise UI

![](.//media/image124.png)

1)  Execute ***step 3*** again. We will perform another deploy. You will
    > notice that the revision number has increased and the latest
    > version has been deployed.

![](.//media/image93.png)

#### Other Build Options

It is possible to build and deploy based on a tag. Here are the steps to
build and deploy tagged versions.

1.  Create a tag

![](.//media/image67.png)

Command:
```
git tag v1.0.0

git tag
```

1.  Push the tag to the remote server

![](.//media/image17.png)

Command:
```
git push origin v1.0.0
```

1.  Find the “maven-deploy-bundle-tag” job

> ![](.//media/image62.png)

1.  Schedule a new job by clicking on

> ![](.//media/image90.png)

1.  Enter the details to deploy your proxy

![](.//media/image80.png)

1.  Ensure the build is successful

![](.//media/image78.png)

### Part 4 - Applying environment specific configurations

A common practice within enterprises is to have different configuration
settings for each environment. For ex: the target URL in dev could be
[*http://dev.backend.com/api*](http://dev.backend.com/api) and the
target URL in prod could be
[*http://prod.backend.com/api*](http://prod.backend.com/api)

This section of the lab will show you how to manage and apply
environment specific values to your proxy. In this example we will
change the target URL.

**Test Target URL**:
[https://api.usergrid.com/demo24/sandbox/hotels](https://api.usergrid.com/demo24/sandbox)

**Prod Target URL**: <https://api.usergrid.com/demo2/sandbox/hotels>

Environment configuration is managed in config.json
(src/gateway/hotelsapi/apiproxy)

Here is a sample config.json
```
{
    "configurations": [
        {
            "name": "test",
            "policies": [],
            "proxies": [],
            "targets": [
                {
                    "name": "default.xml",
                    "tokens": [
                        {
                        "xpath": "/TargetEndpoint/HTTPTargetConnection/URL",
                        "value": "http://api.usergrid.com/demo2/sandbox/hotels"
                        }
                    ]
                }
            ]
        },
        {
            "name": "prod",
            "policies": [],
            "proxies": [],
            "targets": [
                {
                    "name": "default.xml",
                    "tokens": [
                        {
                            "xpath": "/TargetEndpoint/HTTPTargetConnection/URL",
                            "value": "https://api.usergrid.com/deom24/sandbox/hotels"
                        }
                    ]
                }
            ]
        }
    ]
}
```

[**NOTE**: Rename the **config.json.txt** file to **config.json**
so that maven will pick it up. After rename, add and commit to GitHub
like this:

Commands:
```
git add --all

git commit -m 'adding env specific config'

git push origin master
```
]

1)  Run the maven build again with the following settings

![](.//media/image68.png)

NOTE: The profile is set to **prod** and the EDGE\_ENV is set to
**prod**

1)  When deployed, you will see the proxy is deployed to the prod
    > environment with a different endpoint

![](.//media/image100.png)

> **Revision 4** is deployed to test (from the previous run). This
> revision has
> [https://api.usergrid.com/](https://api.usergrid.com/demo24/sandbox)[demo24](https://api.usergrid.com/demo24/sandbox)[/sandbox](https://api.usergrid.com/demo24/sandbox)/hotels
> as the backend.
>
> **Revision 5** is deployed to prod (from the previous run). This
> revision has
> [https://api.usergrid.com/](https://api.usergrid.com/demo24/sandbox)[demo2](https://api.usergrid.com/demo24/sandbox)[/sandbox](https://api.usergrid.com/demo24/sandbox)/hotels
> as the backend.

### Part 5 - Rollback to a previously deployed version of the proxy

1)  Login to Jenkins here:
    > [*http://54.225.22.81:8080/*](http://54.225.22.81:8080/)[](http://54.87.93.40:8080/)

2)  Find the “grunt-restore-build” job

![](.//media/image95.png){width="3.5052088801399823in"
height="0.43206583552055994in"}

1)  Schedule a new job by click on

> ![](.//media/image86.png)

1)  Enter the details for Edge

![](.//media/image98.png)

NOTE: UNDEP\_REV is the version of the API proxy that should be
undeployed. DEP\_REV is the version of the API proxy that should be
deployed.

1)  Click on “Build”

> <span id="h.gjdgxs-1" class="anchor"></span>

1)  Click the build id associated with your task

> ![](.//media/image77.png)

1)  Then select console output to view the build process

![](.//media/image63.png)

![](.//media/image81.png)

1)  Confirm the rollback of the API proxy revision in the Enterprise UI

![](.//media/image69.png)

**NOTE**: The revision for **hotelsapi** in **prod** changed from **5**
to **4**.

Summary
-------

Apigee Edge doesn’t not take an opinionated position on how CI/CD should
happen. Since Apigee’s administrative functions are available themselves
as APIs, it is easy to fit Apigee into most CI/CD models/processes.

Apigee also provides tools based on popular CI/CD tools like Maven and
Grunt and executing these via Jenkins to help teams manage deployment of
API proxies.

In this lab you learnt how to setup an API proxy for use with Maven. You
also learnt how to use Grunt to modify deployments. We used two tools
here not because you need two tools for these tasks, but to show how you
can use a variety of tools to accomplish CI/CD.

Appendix
--------

Please refer to online documentation for how Maven and Grunt (node.js,
npm, grunt and grunt-cli) is installed on RHEL. The installation of
those activities is out of scope of this document.

### Maven Plugin (installed on the Jenkins machine)

Please see installation instructions here:
[*https://github.com/apigee/apigee-deploy-maven-plugin*](https://github.com/apigee/apigee-deploy-maven-plugin)

### Grunt Plugin (installed on the Jenkins machine)

Please see installation instructions here:

[*https://github.com/apigeecs/apigee-deploy-grunt-plugin*](https://github.com/apigeecs/apigee-deploy-grunt-plugin)

### Jenkins Configuration

1.  Go to **Manage Jenkins** -> **Global Tool Configuration** aa

    a.  **Maven Configuration**

> ![](.//media/image87.png)
>
> ![](.//media/image20.png)
> 
> ![](.//media/image89.png)

a.  **Git Configuration**

> ![](.//media/image18.png)

a.  **NodeJS Configuration (for Grunt)**

> ![](.//media/image21.png)

### Jenkins Tasks

-   **maven\_deploy\_bundle**: This task downloads a proxy bundle from
    > GitHub, applies environment specific configuration (if available)
    > and deploys the bundle to an org/environment.

![](.//media/image59.png)

![](.//media/image85.png)

![](.//media/image64.png)

![](.//media/image70.png)

![](.//media/image82.png)

-   **maven\_deploy\_bundle\_tag:** This task downloads a proxy bundle
    that matches a specific tag from GitHib, applies environment
    specific configuration (if available) and deploys the bundle to an
    org/environment

![](.//media/image15.png)

-   **Grunt-restore-build**: This task undeploys a specific revision of
    a proxy and deploys another revision of the same proxy.

![](.//media/image92.png)

![](.//media/image117.png)

![](.//media/image102.png)

![](.//media/image76.png)
