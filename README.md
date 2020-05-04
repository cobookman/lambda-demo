# Lambda@Edge Demo

The git repo includes 2 Cloudformation templates: `edge.yaml` and `region.yaml`.

## Test existing deployment

You can test my already deployed endpoint with curl. For simplicity to see how it chooses between the two lambda endpoints run the following CLI command which extracts the [`Content-Location`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Location) HTTP Header:

```bash
$ curl -I http://d124vwf0s3f67.cloudfront.net/ | grep Content-Location
```

## Setup

1. In two or more AWS regions of your choice, open CloudFormation, and upload the `region.yaml` script. Choose default settings and deploy. Make sure to log the final lambda endpoint URL.

Launch Lambda in us-east-1: [![Step 1a](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=us-east1-lambda&templateURL=https://sumerian-barbarians.s3-us-west-2.amazonaws.com/cloudformation-public/region.yaml)


Launch Lambda in us-west-2: [![Step 1b](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=us-west2-lambda&templateURL=https://sumerian-barbarians.s3-us-west-2.amazonaws.com/cloudformation-public/region.yaml)


2. Modify the `endpoints = [....]` in `edge.yaml` to be a list of these lambda endpoints. Currently set to be two lambda functions from boocolin@amazon's environment. Then using CloudFormation in **us-east1** upload & deploy `edge.yaml`.

Launch Lambda@edge in us-east-1: [![Step 1b](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=edge-lambda&templateURL=https://sumerian-barbarians.s3-us-west-2.amazonaws.com/cloudformation-public/edge.yaml)



## edge.yaml

Contains a lambda function which randomly will proxy data fron an element in `endpoints`. It'll include the original `endpoint` URL in the HTTP Header of 'Content-Location`. This lambda function is deployed directly in a CloudFront PoP

## region.yaml

A lambda function that is deployed into an AWS Region. This lambda is put behind an API Gateway and exposed to the world.

