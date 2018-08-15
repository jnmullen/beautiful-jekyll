---
layout: post
published: true
title: How many ways can you skin infrastructure as code?
---
## How many ways can you skin infrastructure as code?

I've been meaning to look into this for a while now knowing there are a number of frameworks/tools available to achieve infrastructure as code. A new one was released only a few weeks ago by AWS [aws-cdk](https://github.com/awslabs/aws-cdk/) which piqued my interest and made me start this post.

As well as aws-cdk I am going to cover the following alternatives:

- [Terraform](https://www.terraform.io)
- [Serverless Framework](https://serverless.com/)

Maybe in a future blog I will also have a look at:

- [Cloudformation](https://aws.amazon.com/cloudformation/)
- [SAM](https://github.com/awslabs/serverless-application-model)

I'm going to try and create the diagram shown below in the three frameworks:

![AWS Diagram]({{site.baseurl}}/img/lambda-aws.jpeg)

## Terraform

Code can be found on github : [Terraform Example](https://github.com/jnmullen/blog-iac-examples/tree/master/terraform)

Terraform is the technology I know the most about and I do use it in my day job to provision AWS resources. Some people might say it is very verbose but I quite like the fact if you click something in the AWS Console I know I am going to have to write some terraform to create the same resource. There is no magic (in comparison to some of the other frameworks).

In my example I have created a reusable module called __lambda-cloudwatch-trigger__ - the idea being you could then deploy this into multiple environments by creating a set of top level .tf files one per environment. The module has a set of variables in __variables.tf__ which have some sensible defaults but allow you to override these at the point you include the module. There is only one required variable which is the S3 bucket name from which to get the artifact for the Lambda function.

## Serverless Framework

Code can be found on github : [Serverless Framework Example](https://github.com/jnmullen/blog-iac-examples/tree/master/sf-lambda-cloudwatch-triggered)

This seems to be one of the most popular frameworks for people provisioning purely serverless infrastructure. The yml file is pretty simple and self explanatory, I liked the fact I could setup stages/environments pretty easily. I only included one in the file but adding more wouldn't be hard.

There is plenty of magic going on behind the scenes, the input (Cloudwatch Event in this example) is being given permission to invoke your Lambda, a default IAM Role is being created with some basic permissions which I then added to in this example.

I found the environment variable inheritance tricky, I didn't like that I had this in two places but couldn't see how else todo it:

```
${self:custom.ENVIRONMENT_NAME.${self:provider.stage}}
```

Behind the scenes the Serverless Framework is also automatically creating aliases for your Lambda each time you deploy which again would be something you would have to explicitly code for in Terraform or aws-cdk as well from what I can see.

## aws-cdk

Code can be found on github : [aws-cdk Example](https://github.com/jnmullen/blog-iac-examples/tree/master/cdk-example)

This is a really new offering from AWS - basically write some code (Java/Javascript/Typescript) and this is synthised into a Cloudformation script which can be applied/diffed etc.

Things to like here I am just writing code, not having to learn yaml or Terraform resources. It feels quite natural you create a Lambda then create a Cloudwatch Rule and simple add the Lambda as the target. Behind the scenes some magic happens, permission is given for Cloudwatch to invoke your Lambda. I did have to report an issue when I first tried this as it wasn't showing up correctly in the console see [aws-cdk issue 555](https://github.com/awslabs/aws-cdk/issues/555). I raised the issue after chatting on gitter, the issue was fixed the next day and a new version released on npm the day after, quicker than I could get my house in order to get this blog post written!

aws-cdk is really new, there aren't many examples or best practices around. If I am honest I'm not 100% sure how to get a stack to work with multiple environments but that is something to look into in another blog post as this one has taken my longer than I ever thought to put together.
