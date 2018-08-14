---
layout: post
published: false
title: How many ways can you skin infrastructure as code?
---
## How many ways can you skin infrastructure as code?

I've been meaning to look into this are for a while now knowing there are a number of ways of acheiving infrastructure as code I wanted to have a look at:

- [Terraform](https://www.terraform.io)
- [Cloudformation](https://aws.amazon.com/cloudformation/)
- [SAM](https://github.com/awslabs/serverless-application-model)
- [Serverless Framework](https://serverless.com/)
- [aws-cdk](https://github.com/awslabs/aws-cdk)

Last week I was helping a colleague setup a basic Lambda, triggered by a Cloudwatch timer event, it did nothing special apart from having access to ParameterStore to gain access to some config. He was alarmed at the amount of terraform we had to write to get this all setup - this was even with reusable modules existing  for most of the terraform entities we needed to setup.

I wondered how easy/different some of the other ways of setting up IAC were and what +/- each one had hence writing this blog post.

I'm going to attempt to setup the following in each of the above IAC frameworks/languages and see what I can learn from the experience:

![AWS Diagram]({{site.baseurl}}/img/lambda-aws.jpeg)

- Single Node.js Lambda Function
- Cloudwatch Event triggers Lambda daily
- Lambda has permission to call the following:
	- Access SSM/Parameter store on well defined path
	- Write logs to Cloudwatch

I've realised since I started this blog post that trying this in all five technologies might have been a bit optermistic based on the amount of free time I have, therefore I am going to start with Terraform, Serverless Framework and aws-cdk.

## Terraform

Code can be found on github : [Terraform Code](https://github.com/jnmullen/blog-iac-examples/tree/master/terraform)

Terraform is the technology I know the most about and I do use it occasionally in my day job to provision AWS resources. Some people might say it is very verbose but I quite like the fact if you click something in the AWS Console I know I am going to have to write some terraform to create the same resource. There is no magic (in comparison to some of the other later technologies).

In my example I have created in theory a reusable module called __lambda-cloudwatch-trigger__ - the idea being you could then deploy this into multiple environments by creating a set of top level .tf files one per environment. The module has a set of variables in __variables.tf__ which have some sensible defaults but allow you to override these a the point you include the module.


## Serverless Framework
