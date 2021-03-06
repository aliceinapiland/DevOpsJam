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
curl -u {edge_user_email} https://api.enterprise.apigee.com/v1/organizations/{org_name}/apis/{api_proxy_name}
```
**Example Request:**
```
curl -u ravindranv@google.com https://api.enterprise.apigee.com/v1/organizations/demo32/apis/vr_employee_proxy
```

**Response:**
```
Enter host password for user 'ravindranv@google.com':
{
  "metaData" : {
    "createdAt" : 1493722732980,
    "createdBy" : "ravindranv@google.com",
    "lastModifiedAt" : 1493810683513,
    "lastModifiedBy" : "ravindranv@google.com",
    "subType" : "Proxy"
  },
  "name" : "vr_employee_proxy",
  "revision" : [ "1", "2" ]
}
```

You can also try out the APIs here:
[*http://docs.apigee.com/management/apis/get/organizations/%7Borg\_name%7D/apis/%7Bapi\_name%7D*](http://docs.apigee.com/management/apis/get/organizations/%7Borg_name%7D/apis/%7Bapi_name%7D)

**Step 2:** Lets get the details of an environment of an org:

**Request:** 
```
curl -u {edge_user_email} https://api.enterprise.apigee.com/v1/organizations/{org}/e/{env}
```
**Example Request:**
```
curl -u ravindranv@google.com https://api.enterprise.apigee.com/v1/organizations/demo32/e/test
```

**Response:**
```
Enter host password for user 'ravindranv@google.com':
{
  "createdAt" : 1381001690025,
  "createdBy" : "lyeo@apigee.com",
  "lastModifiedAt" : 1464341431388,
  "lastModifiedBy" : "sanjoy@apigee.com",
  "name" : "test",
  "properties" : {
    "property" : [ {
      "name" : "uapEnabled",
      "value" : "true"
    }, {
      "name" : "samplingTables",
      "value" : "10=ten;1=one;"
    }, {
      "name" : "samplingInterval",
      "value" : "300000"
    }, {
      "name" : "offlineQueryEnabled",
      "value" : "true"
    }, {
      "name" : "aggregationinterval",
      "value" : "300000"
    }, {
      "name" : "factquery_spark_enabled",
      "value" : "true"
    }, {
      "name" : "useSampling",
      "value" : "100"
    }, {
      "name" : "samplingAlgo",
      "value" : "reservoir_sampler"
    }, {
      "name" : "samplingThreshold",
      "value" : "100000"
    } ]
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
curl -X POST "https://api.enterpris.apigee.com/v1/organizations/demo32/developers" -H "Content-Type:application/json" -u ravindranv@google.com -d '{"email":"ravindranv+test@google.com","firstName":"Vidya","lastName":"Ravindran","userName":"VidyaTest","attribute":[{"name":"cellphone","value":"4156963692"},{"name":"city","value":"Austin"}]}'
```
**Response:**
```
Enter host password for user 'ravindranv@google.com':
{
  "apps" : [ ],
  "email" : "ravindranv+test@google.com",
  "developerId" : "iVXqpef4oS9YNxo7",
  "firstName" : "Vidya",
  "lastName" : "Ravindran",
  "userName" : "VidyaTest",
  "organizationName" : "demo32",
  "status" : "active",
  "attributes" : [ ],
  "createdAt" : 1494224656310,
  "createdBy" : "ravindranv@google.com",
  "lastModifiedAt" : 1494224656310,
  "lastModifiedBy" : "ravindranv@google.com"
}
```

Or you can use this API from here:
[*http://docs.apigee.com/management/apis/post/organizations/%7Borg\_name%7D/developers*](http://docs.apigee.com/management/apis/post/organizations/%7Borg_name%7D/developers)

**Step 4:** Use Analytics APIs to export Analytics data

**Request:** 
```
curl -X GET "https://api.enterprise.apigee.com/v1/organizations/demo32/environments/test/stats/?select=sum(message_count)&timeRange=05/01/2017%2000:00~05/05/2017%2023:59" -u ravindranv@google.com
```

**Response:**
```
Enter host password for user 'ravindranv@google.com':
{
  "environments" : [ {
    "metrics" : [ {
      "name" : "sum(message_count)",
      "values" : [ "18049.0" ]
    } ],
    "name" : "test"
  } ],
  "metaData" : {
    "errors" : [ ],
    "notices" : [ "query served by:7c9dc2e5-729b-4ce3-b729-ae2f457f22ea", "source pg:ruapdb02r.us-ea.4.apigee.com", "Table used: demo32.test.agg_api" ]
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
