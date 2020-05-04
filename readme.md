# Lambda@Edge Demo

The git repo includes 2 Cloudformation templates: `edge.yaml` and `region.yaml`.

## Setup

1. In two or more AWS regions of your choice, open CloudFormation, and upload the `region.yaml` script. Choose default settings and deploy. Make sure to log the final lambda endpoint URL.
2. Modify the `endpoints = [....]` in `edge.yaml` to be a list of these lambda endpoints. Currently set to be two lambda functions from boocolin@amazon's environment. Then using CloudFormation in **us-east1** upload & deploy `edge.yaml`.


## edge.yaml

Contains a lambda function which randomly will proxy data fron an element in `endpoints`. It'll include the original `endpoint` URL in the HTTP Header of 'Content-Location`. This lambda function is deployed directly in a CloudFront PoP

## region.yaml

A lambda function that is deployed into an AWS Region. This lambda is put behind an API Gateway and exposed to the world.

