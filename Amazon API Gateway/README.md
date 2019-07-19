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
 ```bash {
    "greeting": "Good {time}, {name} of {city}.[ Happy {day}!]"
   } ```
   





