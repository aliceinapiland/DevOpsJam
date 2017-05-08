#Lab 2 - Other Utilities

##Overview

There are many open source tools in the Apigee eco system. We have so far covered the most important ones. But there are many other tools which you might find useful. We have covered here some of those tools.

##Objectives

We are going to use the following open source tools to do various tasks:

###Migration Tool

####Objectives

If you have any of the following use cases you will need this tool:

*   Migrate apigee from cloud to on-prem or vice versa
*   Import existing API keys
*   Migrate from Trial to any paid plan 

This is a node based tool. We have already installed node as part of Lab 2. 

####Installation

The org migration tool is located at:
[*https://github.com/apigeecs/apigee-migrate-tool*](https://github.com/apigeecs/apigee-migrate-tool)

Download the zip file from here:
[*https://github.com/apigeecs/apigee-migrate-tool/archive/master.zip*](https://github.com/apigeecs/apigee-migrate-tool/archive/master.zip)

Or if you have git installed run a git clone command:
```
git clone https://github.com/apigeecs/apigee-migrate-tool.git
```

####Usage

Once you have the project downloaded then go to the directory named: apigee-migrate-tool


Command: 
```
cd apigee-migrate-tool
```

Let's configure the tool to work with our org/environment. We will modify the config.js file.

```
module.exports = {
 from: {
 url: 'https://api.enterprise.apigee.com/',
 userid: '{username}',
 passwd: '{password}',
 org: '{devjam\_org\_name}',
 env: 'test'
 },
 to: {
 url: 'https://api.enterprise.apigee.com/',
 userid: '{username}',
 passwd: '{password}',
 org: '{orgName}',
 env: '{environmentName}'
 }
};
```

As you can see we have just modified the “from” field and deleted the first element “version”.


Let's export a number of products:


Command: 
```
grunt exportProducts *
```

Go to folder: data -> Products


You will find Json representation of all the API products in your org


###Generate and Deploy API Proxy from OpenAPI Spec


####Objectives

OpenAPI is fast developing as Open Standard and there is an increased developer adoption. If you follow the design first approach and have the OpenAPIs , you can use this tool to generate api proxy from OpenAPIs specification.

####Installation
```
sudo npm install -g openapi2apigee
```

Example

Command:
```
swagger2api generateApi hotelsapi -s
https://raw.githubusercontent.com/apigee/DevJam/master/Resources/hotels-openapi.json
-D -d .*
```

![](.//media/image08.png)

Enter details as shown in the picture

This will create the apigee api proxy.


###Deploy In Apigee from Git repo without any tools

####Objectives

There are many apigee tools available to deploy apiproxies and do life cycle management . However if you don’t want to install any tools locally there is a way to it . Say you have a github repo with all apiproxies created and you want to deploy them to edge , this tool comes handy

####Usage

* Click on the link below

[*https://github.com/apigee/apigee-deploy-now*](https://github.com/apigee/apigee-deploy-now)

* Click on Deploy To Apigee Button in the github. This will bring apage like this.
    ![](.//media/image06.png)

* Change the url in the top to your github repo and refresh the page

[*https://deploynow.apigee.com/login-form/?repo=https://github.com/akoo1010/apigee-nock-mock-deploy-now.git&apiFolder=/&makeScript=make.sh*](https://deploynow.apigee.com/login-form/?repo=https://github.com/akoo1010/apigee-nock-mock-deploy-now.git&apiFolder=/&makeScript=make.sh)

Change the
[*https://github.com/akoo1010/apigee-nock-mock-deploy-now.gi*](https://deploynow.apigee.com/login-form/?repo=https://github.com/akoo1010/apigee-nock-mock-deploy-now.git&apiFolder=/&makeScript=make.sh)t
to your git repository and refresh.

Provide Edge Credentials and hit Deploy Now to deploy in edge


###ApigeeDM - Deploy multiple proxies from command line


It allows you to deploy multiple proxies by executing a simple command at one go. You don't need to execute apigee deploy command multiple times for multiple proxies individually.

####Installation


Installing this plugin is super smooth. Just execute below command to install apigeedm plugin. Make sure Node.JS & NPM is installed in your machine.
```
sudo npm install -g apigeedm
```

####Usage

You will just specify folder location where your apigee proxies are present & apigee credentials, rest tool will take care to deploy multiple proxis in one go. You need to make sure below folder structure of source location that you specify.

Let's say your root directory is **/Users/Anil/Desktop/multipleProxies** where multiple proxies are present. Let's say we have proxies **weather1** & **weather2**. Directory structure should be something like below.

Directory Structure should look like below:


![](.//media/image09.png)

![](.//media/image04.png)

Create a directory on your local filesystem like 'apigeedm' or something descriptive. unpack with tar - xvf ./apigeedm.tar. The proxies subdirectory contains two separate API proxies that we wish to deploy, each with their own top level directory. You can replace the ones I included with anything else, just create a directory for each one.


Execute below command with source directory option to deploy multiple
proxies present in same folder:

```
apigeedm -s \~/Desktop/multipleProxies
```

That's it, multiple proxies will be deployed to Apigee Edge at one go.

You can also provide org credentials directly while executing the
command itself using options provided by command line. It will be
helpful if you would like to use this tool in some other script.

```
apigeedm -s \~/Desktop/multipleProxies -b
https://api.enterprise.apigee.com -o ORGNAME -u USERNAME -p PASSWORD -e
ENVNAME -v VIRTUALHOSTS
```
Full documentation is @:
[*https://www.npmjs.com/package/apigeedm*](https://www.npmjs.com/package/apigeedm)


###Clean Org

####Objective
If you have an existing org and you want to wipe it clean then you can follow the steps below:

* It deletes all developers
* Deletes all apps
* Deletes all API Products
* Undeployes all all API proxies from any environment
* Deletes all API Proxies
* Removes KVMs,vaults,custom resports

[*https://github.com/DinoChiesa/EdgeTools/blob/master/wipe-org/wipe-org.sh*](https://github.com/DinoChiesa/EdgeTools/blob/master/wipe-org/wipe-org.sh)


##Appendix

You will find a lot of other open source tools here:
[*https://github.com/DinoChiesa/EdgeTools*](https://github.com/DinoChiesa/EdgeTools)

[*https://github.com/anil614sagar*](https://github.com/anil614sagar)

[*http://apigee.com/about/labs*](http://apigee.com/about/labs)

