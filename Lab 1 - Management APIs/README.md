#Lab 1 -  Management APIs

##Overview

You may have used the EDGE UI to build API Proxy, deploy proxies, manage
developers, apps, keys, products etc. But many customers want to
automate all that management and don’t want to do it via the UI.

All Apigee Edge services can be configured through a RESTful API called
the Edge management API. That means you can use scripts to create,
configure, and manage API proxies and API products, create and manage
apps and app developers, and to perform many other types of operations
programmatically, using any HTTP client. The API resources exposed by
the Edge management API support JSON and XML, and are secured via HTTP
Basic Authentication. You need to provide the email address and password
associated with your organization on Apigee Edge with each API call that
you make.

Following RESTful principles, you can call HTTP GET, POST, PUT, and
DELETE methods on any of the API resources.

##Objectives

In this lab you will get a familiarity with using the management APIs.
You will understand the basic structure of the APIs, how to authenticate
and how to manage various resources using the APIs.

##Lab Prerequisites

Must have finished lab 0.

You must have some rest client - like curl, postman, rest-client to
finish the lab successfully. If you do not have any rest client
installed and only have your browser then you will be able to do the
labs partially.

##Lab Steps

Authentication: Use your email address and password which you use to
login to Apigee Edge.

Note: The permissions for using the APIs are controlled by the RBAC
rules as set in the UI.

**Step 1:** Let’s use the APIs to GET an API Proxy.

**Request:** 
```
curl -u {username} https://api.enterprise.apigee.com/v1/organizations/{org_name}/apis/hotelsapi
```

**Response:**
```
{
"metaData" : {
"createdAt" : 1453737784596,
"createdBy" : "sarthak@apigee.com",
"lastModifiedAt" : 1453737784596,
"lastModifiedBy" : "sarthak@apigee.com"
},
"name" : "hotels",
"revision" : [ "1" ]
}
```

You can also try out the APIs here:
[*http://docs.apigee.com/management/apis/get/organizations/%7Borg\_name%7D/apis/%7Bapi\_name%7D*](http://docs.apigee.com/management/apis/get/organizations/%7Borg_name%7D/apis/%7Bapi_name%7D)

**Step 2:** Lets get the details of an environment of an org:

**Request:** 
```
curl -u {username} https://api.enterprise.apigee.com/v1/organizations/{org}/e/{env}
```

**Response:**
```
{
"createdAt" : 1428636004646,
"createdBy" : "noreply_admin@apigee.com",
"lastModifiedAt" : 1428636004646,
"lastModifiedBy" : "noreply_admin@apigee.com",
"name" : "test",
"properties" : {
"property" : [ {
"name" : "useSampling",
"value" : "100"
}, {
"name" : "samplingThreshold",
"value" : "100000"
}, {
"name" : "samplingTables",
"value" : "1=one;"
}, {
"name" : "samplingAlgo",
"value" : "reservoir_sampler"
}, {
"name" : "samplingInterval",
"value" : "300000"
}, {
"name" : "aggregationinterval",
"value" : "300000"
}]
}
}
```

Or try out the API here:
[*http://docs.apigee.com/management/apis/get/organizations/%7Borg\_name%7D/environments/%7Benv\_name%7D*](http://docs.apigee.com/management/apis/get/organizations/%7Borg_name%7D/environments/%7Benv_name%7D)

**Step 3:** Let’s use this API to create a Developer

**Note:** This is the same API which developer portal uses to create a
developer. You can use this API to integrate developer creation to any
of your portals or internal systems.

**Request:** 
```
curl -X POST --header "Content-Type: application/json" --header "Authorization: Basic abcdxyz==" -d '{
"email" : "goose_ipa@gmail.com",
"firstName" : "goose",
"lastName" : "IPA",
"userName" : "gooseipa",
"attributes" : [
{
"name" : "telephone",
"value" : "9179326040"
},
{
"name" : "city",
"value" : "nyc"
}
]
}'
```


[*https://api.enterprise.apigee.com/v1/organizations/pixvy/developers*](https://api.enterprise.apigee.com/v1/organizations/pixvy/developers)

Or you can use this API from here:
[*http://docs.apigee.com/management/apis/post/organizations/%7Borg\_name%7D/developers*](http://docs.apigee.com/management/apis/post/organizations/%7Borg_name%7D/developers)

**Step 4:** Use Analytics APIs to export Analytics data

**Request:** 
```
curl -X GET --header "Authorization: Basic ABCDXYZ123="
"https://api.enterprise.apigee.com/v1/organizations/{org_name}/environments/{env}/stats/?select=sum(message_count)&timeRange=03%2F01%2F2016%2000%3A00\~03%2F31%2F2016%2024%3A00"
```

**Response:**
```
{
"environments": [
{
"metrics": [
{
"name": "sum(message_count)",\
"values": [
"2706.0"
]
}
],
"name": "test"
}
],
"metaData": {
"errors": [],
"notices": [
"source pg:28413fc7-b6de-43a9-af35-fbd49f11516f",
"Table used: pixvy.test.agg_api",
"query served by:ff0ec974-421b-4801-9074-4f9d1ca81fe2"
]
}
}
```

Or use the APIs here:
[*http://docs.apigee.com/management/apis/get/organizations/%7Borg\_name%7D/stats*](http://docs.apigee.com/management/apis/get/organizations/%7Borg_name%7D/stats)

**Misc:**

There is a node.js SDK to work with the management APIs. You can learn
more about the SDK here:
[*https://www.npmjs.com/package/apigee-sdk-mgmt-api*](https://www.npmjs.com/package/apigee-sdk-mgmt-api).

There is also a postman button to get many of these APIs as a postman
collection. You can find details here:
[*https://github.com/apigee/apigee-management-api-postman*](https://github.com/apigee/apigee-management-api-postman)
