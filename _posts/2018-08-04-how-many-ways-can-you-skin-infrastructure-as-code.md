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

- Single Node.js Lambda Function
- Cloudwatch Event triggers Lambda daily
- Lambda has permission to call the following:
	- Access SSM/Parameter store on well defined path
	- Write logs to Cloudwatch