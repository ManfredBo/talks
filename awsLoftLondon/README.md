# AWS Pop-up Loft Workshop 
# Designing and managing scalable APIs with AWS and 3scale

This workshop is jointly delivered between Amazon Web Services (AWS) and the AWS Advanced Technology partner 3scale API Management Platform. Together we provide a full complement API program management solution that integrates 3scale with the Amazon API Gateway and Lambda.

The workshop is held at the AWS Pop-up Loft London on April 25th, 9:00AM - 1:00PM. The workshop description can be found [here](https://awsloft.london/session/2016/fd3f2e85-b292-44cd-867d-2c0528cbd741).

This part of the tutorial focuses on how the [integration](https://www.3scale.net/amazon-gateway-integration/) between 3scale, Amazon API Gateway and Lambda can be achieved practically. 

## Table of Contents

* Intro to [3scale](https://www.3scale.net/) API Management
* Goals of this tutorial 
* Prerequisites for this tutorial
* Setting up the Amazon Virtual Private Cloud (VPC)
* Setting up Elasticache
* Creating the Lambda code
* Intro to the Amazon API Gateway [custom authorizer](http://docs.aws.amazon.com/apigateway/latest/developerguide/use-custom-authorizer.html#api-gateway-custom-authorization-overview) principles
* Create and deploy the 3scale-specific custom authorizer

## Intro to 3scale API Management
3scale makes it easy to open, secure, manage, distribute, control, and monetize your APIs. Built with performance, customer control and excellent time-to-value in mind, no other solution gives API providers so much power, ease, flexibility and scalability in such a cost effective way. Check it out at https://www.3scale.net

3scale’s API Management Platform also supports the unique requirements of delivering APIs on the Amazon Web Services (AWS) infrastructure stack -- fexibly, at scale and with great RoI. API providers on AWS don’t have to switch solutions to get Amazon API gateway features like distributed denial-of-service (DDoS) attack protection, caching and logging. Plus, adding 3scale provides rich, sophisticated API management business operations for fine-grained API control and visibility, as well as features for API adoption and promotion. Check out the details about this [integrated solution](https://www.3scale.net/amazon-gateway-integration/).

## Goals of this tutorial 
We've seen the importance of API management solution when you are developping APIs. We will now add an API management layer to our existing API. This existing API has been deployed using the AWS API Gateway.

For this integration we will use:

* API Gateway (core of the API)
* AWS Lamda (for the logic of the "proxy")
* Elasticache (to cache api keys)
* VPC (to connect AWS Lambda and Elasticache)
* Serverless framework (to deploy easily to Lambda)

Here is a graph describing the integration.

And another one more specific to 3scale plugin
showcase connection to elastic search, emphasize VPC

##Prerequisites for this tutorial
* 3scale account – sign up at [3scale.net](https://www.3scale.net/) `TODO: add specific signup URL landing page`
* AWS account – sign up at http://aws.amazon.com
* AWS command line interface (CLI) installed locally -- ([Instructions](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html))
* Node.js environment installed locally -- ([Instructions](https://docs.npmjs.com/getting-started/installing-node))
* Serverless framework installed locally -- ([Instructions](https://github.com/serverless/serverless)) 

##Setting up the Amazon Virtual Private Cloud (VPC)
To reduce latency and have a system that will handle the load of thousands requests, we will use elasticache. There we will store API keys that were authorized to make request to the API. This will help to reduce the calls to the main 3scale platform.

Elasticache is only available through VPC. To be able to call Elasticache into Lambda, the Lambda function has to be on the same VPC.

Our Lambda will make calls to 3scale service, which is outside of the VPC. We will now configure the VPC to make sure it can connect to the internet, outside of VPC.

If don't have a VPC, let's create one. You can also use the default one.

You should create a NAT gateway and an Internet gateway.
Then create a new route tables.
Once the route table is created let's edit the routes.
You should make `0.0.0.0/0` point to the NAT gateway you created.

We will now attach this rule to a subnet in your VPC. Select an existing subnet. On the route tables tab, select the route table you just created.

And that's it for the VPC part. You know have a VPC, that's know connected to the Internet. We will see later how to put Elasticache and Lambda on this VPC.

##Setting up Elasticache
Elasticache is a service offered by AWS to simply cache stuff and access it quickly. It supports both Memcached and Redis. In our example we will use Redis.

Go on Elasticache section in your AWS console and create a Redis cluster under the VPC you defined before. You don't specially need replication for this tutorial.
There is no more setup to do on the Elasticache cluster. 
Once the cluster is ready, go on the node created, and get the Endpoint URL. We will need it later on in the tutorial.

##Creating the Lambda code
We will now work on the Lambda side of the integration. This is where the logic of the custom authorizer will be defined. 
This Lambda function will call 3scale to authorize access to the API.

We will be using Serverless framework to deploy Lambda functions easily.

##Intro to the Amazon API Gateway custom authorizer principles
`TODO`


##Create and deploy the 3scale-specific custom authorizer
`TODO`
