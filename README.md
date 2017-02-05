# aws-infra

Continuous Deployment (CD) Infrastructure code/examples.

Changes are deployed automatically using the CodeDeploy pipeline from GitHub.
If changes need to be trialed before committing, use the following script:

```bash
#!/bin/bash

aws cloudformation deploy \
    --stack-name iot \
    --template-file $(readlink -f iot.yaml) \
    --capabilities CAPABILITY_NAMED_IAM
```

## IoT - Alexa Lambda function and IoT message queue

Supports voice requests sent from an Alexa device such as an Amazon Echo, processing those requests in a Lambda function and
sending commands down an MQTT queue using AWS IoT. 

This CloudFormation template is stand-alone, except for the IoT certificate.

The device end of the AWS IoT queue is a Raspberry Pi 3 running [music visualization](https://github.com/scottlawsonbc/audio-reactive-led-strip)
software with some modifications to connect to the AWS IoT endpoint and receive visualization commands. 
None of the device software is provided in this repo.

The Alexa skill itself has to be created manually - there is currently no scriptable API to 
register Alexa skills. The files used in the registration process are in ```ReactiveLedsSkill```.

## Zimicats - Backup location for a minecraft server

Provides S3 bucket with lifecycle policy for a minecraft server (not on AWS) to 
backup worlds.

## Pipeline - CodeDeploy Pipeline

A CodeDeploy pipeline to monitor this GitHub repo and automatically deploy changes
using the other CloudFormation templates to AWS.

The GitHub oauth token should be added to this directory in ```github_oauth_token```.

```deploy-stack-builder``` is used to initiate a deployment of this pipeline. 
It's Turtles All The Way Down, but only up to a point! Those chickens and eggs get in the way.

