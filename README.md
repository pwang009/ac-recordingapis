
# Amazon Connect recording API's demo  

This demo shows how you can leverage [Amazon Connect](https://aws.amazon.com/connect/) api's to start/stop/pause/resume call recordings when the agent is on call with the customer.  These api's come very handy, when accepting PCI/PII type of data from the customers, where agent can manually pause/resume or programatically pause/resume the recording. The sample html page demo's shows both versions. 

## Reference
https://aws.amazon.com/blogs/contact-center/pausing-and-resuming-call-recordings-with-a-new-api-in-amazon-connect/

## Usage
Ensure you are under directory "recording-lambda"
`cd recording-lambda`

Run this command to init npm
`npm init`

Run this command to install the latest version aws-sdk and nodejs modules
`npm install --save aws-sdk`

Use `sam` to build, invoke and deploy the function.

##### SAM Build:
Ensure you are in the root folder

`sam build --use-container`

##### SAM Deploy:
`sam deploy template.yaml --s3-bucket REPLACE_ME --stack-name REPLACE_ME --parameter-overrides ParameterKey=CADS3BucketForWebSite,ParameterValue=REPLACE_ME ParameterKey=instanceIdParam,ParameterValue=REPLACE_ME_WITH_AMAZON_CONNECT_INSTANCE_ID ParameterKey=instanceNameParam,ParameterValue=REPLACE_ME_WITH_INSTANCE_ALIAS --capabilities CAPABILITY_IAM`
      
##### SAM Invoke:
`sam local invoke "CADCallRecordingsControlLambdaFunction" -t template.yaml -e events/resume.json --env-vars events/env.json` 

Note:
Validate the Amazon Connect instance name is like xxxxx.my.connect.aws.  If it is like awsapps.com, you can ignore the following required change.  

If you’re NOT running the Amazon Connect instance in the us-east-1 region and domain name is like my.connect.aws, it is required to change agentRecording.html file from line 191 to 195.  Change awsapps.com to my.connect.aws, like below 

from: 
var loginURL = `https://${instanceAlias}.awsapps.com/connect/login`;
var logoutURL = `https://${instanceAlias}.awsapps.com/connect/logout`;
var instanceHome = `https://${instanceAlias}.awsapps.com/connect/home`;
var ccpURL = `https://${instanceAlias}.awsapps.com/connect/ccp-v2`;
var agentMetricsURL = `https://${instanceAlias}.awsapps.com/connect/real-time-metrics?tableType=user`;

to:
var loginURL = `https://${instanceAlias}.my.connect.aws/connect/login`;
var logoutURL = `https://${instanceAlias}.my.connect.aws/connect/logout`;
var instanceHome = `https://${instanceAlias}.my.connect.aws/connect/home`;
var ccpURL = `https://${instanceAlias}.my.connect.aws/connect/ccp-v2`;
var agentMetricsURL = `https://${instanceAlias}.my.connect.aws/connect/real-time-metrics?tableType=user`;
