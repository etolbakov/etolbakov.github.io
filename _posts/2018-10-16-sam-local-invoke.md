---
layout: post
feature-img: "assets/img/serverless.png"
title: Unit toxiproxy.service not found
tags: [serverless, lambda, amazon, integration, test, code-pipeline]
---
The problem : need to create IT for a lambda
Prerequisites : a serverless project written in Kotlin (java8 runtime for lambdas). 

[.... some testing scenario details]

[.... main function] : `sam local invoke "MyLambdaFunction" --params`
it is used for local testing -> why (docker image emulates...)

[.... side note] why sls local invoke didn't work for me 
ref to github: https://github.com/serverless/serverless/issues/4867

[.... scenario in code pipeline]

[.... downside] output is not that handy.
Workaround - use grep + jq and bash for asserts (because pipeline needs status)


 