---
layout: post
feature-img: "assets/img/sls-logo.jpeg"
title: Integration test for AWS lambda
tags: [serverless, lambda, amazon, integration, test, code-pipeline]
---
**The problem** : create IT for a lambda
**Prerequisites** : 
 - a serverless project written in Kotlin (java8 runtime for lambdas).
 - aws sam cli
 - docker  

**Scenario** is pretty straightforward: we invoke our lambda handler and assert its response. 
In order to achieve that we would use [The AWS Serverless Application Model](https://docs.aws.amazon.com/lambda/latest/dg/serverless_app.html), more specifically
**AWS SAM Local** feature. The most important benefit which `sam local` provides is the possibility 
to run your handler locally in the environment which is extremely close to the one which AWS creates on the console.
This achieved by a customized Docker image. So you need install [Docker](https://docs.docker.com/install/) first to proceed. Luckily it's easy task to do nowadays. 
   

[.... main function] : `sam local invoke "MyLambdaFunction" --params`
it is used for local testing -> why (docker image emulates...)

[.... side note] why sls local invoke didn't work for me 
ref to github: https://github.com/serverless/serverless/issues/4867

[.... scenario in code pipeline]

[.... downside] output is not that handy.
Workaround - use grep + jq and bash for asserts (because pipeline needs status)


 