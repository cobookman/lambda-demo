# Lambda@Edge Demo

The git repo includes 2 Cloudformation templates: `edge.yaml` and `region.yaml`.

## Test existing deployment

You can test my already deployed endpoint with curl. For simplicity to see how it chooses between the two lambda endpoints run the following CLI command which extracts the [`Content-Location`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Location) HTTP Header:

```bash
$ curl -I http://d124vwf0s3f67.cloudfront.net/ | grep Content-Location
```

## Setup

1. In two or more AWS regions of your choice, open CloudFormation, and upload the `region.yaml` script. Choose default settings and deploy. Make sure to log the final lambda endpoint URL.
2. Modify the `endpoints = [....]` in `edge.yaml` to be a list of these lambda endpoints. Currently set to be two lambda functions from boocolin@amazon's environment. Then using CloudFormation in **us-east1** upload & deploy `edge.yaml`.


## edge.yaml

Contains a lambda function which randomly will proxy data fron an element in `endpoints`. It'll include the original `endpoint` URL in the HTTP Header of 'Content-Location`. This lambda function is deployed directly in a CloudFront PoP

## region.yaml

A lambda function that is deployed into an AWS Region. This lambda is put behind an API Gateway and exposed to the world.

