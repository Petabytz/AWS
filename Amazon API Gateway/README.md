## What Is Amazon API Gateway?

Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST and WebSocket APIs at any scale.
API developers can create APIs that access AWS or other web services as well as data stored in the AWS Cloud. 
As an API Gateway API developer, you can create APIs for use in your own client applications (apps). 
Or you can make your APIs available to third-party app developers. For more information, see Who Uses API Gateway?.

API Gateway creates REST APIs that:

   * Are HTTP-based.

   * Adhere to the REST protocol, which enables stateless client-server communication.

   * Implement standard HTTP methods such as GET, POST, PUT, PATCH and DELETE.

For more information about API Gateway REST APIs, see Use API Gateway to Create REST APIs and Creating a REST API in Amazon API Gateway.

API Gateway creates WebSocket APIs that:

   * Adhere to the WebSocket protocol, which enables stateful, full-duplex communication between client and server.

   * Route incoming messages and based on message content.

For more information about API Gateway WebSocket APIs, see Use API Gateway to Create WebSocket APIs and About WebSocket APIs in API Gateway. 

## Build a Hello World API with Lambda Proxy Integration 

Lambda proxy integration is a lightweight, flexible API Gateway API integration type that allows you to integrate an API method – or an entire API – with a Lambda function. The Lambda function can be written in any language that Lambda supports. Because it's a proxy integration, you can change the Lambda function implementation at any time without needing to redeploy your API.

In this tutorial, you do the following:

 * Create a "Hello, World!" Lambda function to be the backend for the API.

  * Create and test a "Hello, World!" API with Lambda proxy integration.
  
## Topics

   * Create a "Hello, World!" Lambda Function
   * Create a "Hello, World!" API
   * Deploy and Test the API
   
   ### Create a "Hello, World!" Lambda Function

   This function returns a greeting to the caller as a JSON object in the following format:
 ```bash 
 {
    "greeting": "Good {time}, {name} of {city}.[ Happy {day}!]"
   }
   ```
   
### Create a "Hello, World!" Lambda function in the Lambda console

   1. Sign in to the Lambda console at https://console.aws.amazon.com/lambda.

   2. On the AWS navigation bar, choose a region (for example, US East (N. Virginia)).

   Note

   3. Note the region where you create the Lambda function. You'll need it when you create the API.

   4. Choose Functions in the navigation pane.

   5. Choose Create function.

   6. Choose Author from scratch.

   7.  Under Basic information, do the following:

        a.In Function name, enter GetStartedLambdaProxyIntegration.

        b.From the Runtime dropdown list, choose Node.js 8.10.

        c. Under Permissions, expand Choose or create an execution role. From the Execution role dropdown list,choose Create new  role from AWS policy templates.

        d.In Role name, enter GetStartedLambdaBasicExecutionRole.

        e. Leave the Policy templates field blank.

        f. Choose Create function.

   7. Under Function code, in the inline code editor, copy/paste the following code:
   ```bash
   'use strict';
console.log('Loading hello world function');
 
exports.handler = async (event) => {
    let name = "you";
    let city = 'World';
    let time = 'day';
    let day = '';
    let responseCode = 200;
    console.log("request: " + JSON.stringify(event));
    
    if (event.queryStringParameters && event.queryStringParameters.name) {
        console.log("Received name: " + event.queryStringParameters.name);
        name = event.queryStringParameters.name;
    }
    
    if (event.queryStringParameters && event.queryStringParameters.city) {
        console.log("Received city: " + event.queryStringParameters.city);
        city = event.queryStringParameters.city;
    }
    
    if (event.headers && event.headers['day']) {
        console.log("Received day: " + event.headers.day);
        day = event.headers.day;
    }
    
    if (event.body) {
        let body = JSON.parse(event.body)
        if (body.time) 
            time = body.time;
    }
 
    let greeting = `Good ${time}, ${name} of ${city}.`;
    if (day) greeting += ` Happy ${day}!`;

    let responseBody = {
        message: greeting,
        input: event
    };
    
    // The output from a Lambda proxy integration must be 
    // in the following JSON object. The 'headers' property 
    // is for custom response headers in addition to standard 
    // ones. The 'body' property  must be a JSON string. For 
    // base64-encoded payload, you must also set the 'isBase64Encoded'
    // property to 'true'.
    let response = {
        statusCode: responseCode,
        headers: {
            "x-custom-header" : "my custom header value"
        },
        body: JSON.stringify(responseBody)
    };
    console.log("response: " + JSON.stringify(response))
    return response;
};
```
8.Choose Save. 

### Create a "Hello, World!" API

Now create an API for your "Hello, World!" Lambda function by using the API Gateway console.

Build a "Hello, World!" API

  1.Sign in to the API Gateway console at https://console.aws.amazon.com/apigateway.

  2. If this is your first time using API Gateway, you see a page that introduces you to the features of the service. Choose Get Started. When the Create Example API popup appears, choose OK.If this is not your first time using API Gateway, choose Create API.
  3. Create an empty API as follows:
   a. Under Choose the protocol, choose REST.
   b. Under Create new API, choose New API.
   c.Under Settings:

  * For API name, enter LambdaSimpleProxy.

  * If desired, enter a description in the Description field; otherwise, leave it empty.

   * Leave Endpoint Type set to Regional.

   d. Choose Create API.

   4. Create the helloworld resource as follows:

        * Choose the root resource (/) in the Resources tree.

        * Choose Create Resource from the Actions dropdown menu.

        * Leave Configure as proxy resource unchecked.

        * For Resource Name, enter helloworld.

        * Leave Resource Path set to /helloworld.

        * Leave Enable API Gateway CORS unchecked.

        * Choose Create Resource.
        
5. In a proxy integration, the entire request is sent to the backend Lambda function as-is, via a catch-all ANY method that represents any HTTP method. The actual HTTP method is specified by the client at run time. The ANY method allows you to use a single API method setup for all of the supported HTTP methods: DELETE, GET, HEAD, OPTIONS, PATCH, POST, and PUT.

    To set up the ANY method, do the following:

    * In the Resources list, choose /helloworld.

    * In the Actions menu, choose Create method.

    * Choose ANY from the dropdown menu, and choose the checkmark icon

    * Leave the Integration type set to Lambda Function.

    * Choose Use Lambda Proxy integration.

    *  From the Lambda Region dropdown menu, choose the region where you created the GetStartedLambdaProxyIntegration Lambda function.

    * In the Lambda Function field, type any character and choose GetStartedLambdaProxyIntegration from the dropdown menu.

     * Leave Use Default Timeout checked.

      * Choose Save.

      * Choose OK when prompted with Add Permission to Lambda Function.
      
      Deploy and Test the API

Deploy the API in the API Gateway console

    Choose Deploy API from the Actions dropdown menu.

    For Deployment stage, choose [new stage].

    For Stage name, enter test.

    If desired, enter a Stage description.

    If desired, enter a Deployment description.

    Choose Deploy.

    Note the API's Invoke URL.

Use Browser and cURL to Test an API with Lambda Proxy Integration

You can use a browser or cURL to test your API.

To test GET requests using only query string parameters, you can type the URL for the API's helloworld resource into a browser address bar. For example: https://r275xc9bmd.execute-api.us-east-1.amazonaws.com/test/helloworld?name=John&city=Seattle

For other methods, you must use more advanced REST API testing utilities, such as ## POSTMAN or ## cURL. This tutorial uses cURL. The cURL command examples below assume that cURL is installed on your computer.

## To test the deployed API using cURL:
  * Open a terminal window.
   * Copy the following cURL command and paste it into the terminal window, replacing r275xc9bmd with your API's API ID and us-east-1 with the region where your API is deployed. 
```bash
curl -v -X POST \
  'https://r275xc9bmd.execute-api.us-east-1.amazonaws.com/test/helloworld?name=John&city=Seattle' \
  -H 'content-type: application/json' \
  -H 'day: Thursday' \
  -d '{ "time": "evening" }'
  ```
  ## Note
  If you're running the command on Windows, use this syntax instead: 
  ``` bash
  curl -v -X POST "https://r275xc9bmd.execute-api.us-east-1.amazonaws.com/test/helloworld?name=John&city=Seattle" -H "content-type: application/json" -H "day: Thursday" -d "{ \"time\": \"evening\" }"
  ```
  You should get a successful response with a payload similar to the following: 
  ``` bash
  {
  "message":"Good evening, John of Seattle. Happy Thursday!", 
  "input":{
    "resource":"/helloworld",
    "path":"/helloworld",
    "httpMethod":"POST",
    "headers":{"Accept":"*/*",
    "content-type":"application/json",
    "day":"Thursday",
    "Host":"r275xc9bmd.execute-api.us-east-1.amazonaws.com",
    "User-Agent":"curl/7.64.0",
    "X-Amzn-Trace-Id":"Root=1-1a2b3c4d-a1b2c3d4e5f6a1b2c3d4e5f6",
    "X-Forwarded-For":"72.21.198.64",
    "X-Forwarded-Port":"443",
    "X-Forwarded-Proto":"https"},
    "multiValueHeaders":{"Accept":["*/*"],
    "content-type":["application/json"],
    "day":["Thursday"],
    "Host":["r275xc9bmd.execute-api.us-east-1.amazonaws.com"],
    "User-Agent":["curl/0.0.0"],
    "X-Amzn-Trace-Id":["Root=1-1a2b3c4d-a1b2c3d4e5f6a1b2c3d4e5f6"],
    "X-Forwarded-For":["11.22.333.44"],
    "X-Forwarded-Port":["443"],
    "X-Forwarded-Proto":["https"]},
    "queryStringParameters":{"city":"Seattle",
    "name":"John"
  },
  "multiValueQueryStringParameters":{
    "city":["Seattle"],
    "name":["John"]
  },
  "pathParameters":null,
  "stageVariables":null,
  "requestContext":{
    "resourceId":"3htbry",
    "resourcePath":"/helloworld",
    "htt* Connection #0 to host r275xc9bmd.execute-api.us-east-1.amazonaws.com left intact pMethod":"POST",
    "extendedRequestId":"a1b2c3d4e5f6g7h=",
    "requestTime":"20/Mar/2019:20:38:30 +0000",
    "path":"/test/helloworld",
    "accountId":"123456789012",
    "protocol":"HTTP/1.1",
    "stage":"test",
    "domainPrefix":"r275xc9bmd",
    "requestTimeEpoch":1553114310423,
    "requestId":"test-invoke-request",
    "identity":{"cognitoIdentityPoolId":null,
      "accountId":null,
      "cognitoIdentityId":null,
      "caller":null,
      "sourceIp":"test-invoke-source-ip",
      "accessKey":null,
      "cognitoAuthenticationType":null,
      "cognitoAuthenticationProvider":null,
      "userArn":null,
      "userAgent":"curl/0.0.0","user":null
    },
    "domainName":"r275xc9bmd.execute-api.us-east-1.amazonaws.com",
    "apiId":"r275xc9bmd"
  },
  "body":"{ \"time\": \"evening\" }",
  "isBase64Encoded":false
  }
}
```
To test the GET method, copy the following cURL command and paste it into the terminal window, replacing r275xc9bmd with your API's API ID and us-east-1 with the region where your API is deployed. 
``` bash
curl -X GET \
  'https://r275xc9bmd.execute-api.us-east-1.amazonaws.com/test/helloworld?Seattle?name=John' \
  -H 'content-type: application/json' \
  -H 'day: Thursday'
  ```  
You should get a response similar to the result from the preceding POST request, except that the GET request does not have any payload. So the body parameter will be null. 



