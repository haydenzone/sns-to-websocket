# sns-to-websocket
Simple serverless application that takes sns notifications and sends them to websocket connections

# Deploying to your account

Install the [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) and use it to package, deploy, and describe your application.  These are the commands you'll need to use:

```
sam package \
	--template-file template.yaml \
	--output-template-file packaged.yaml \
	--s3-bucket REPLACE_THIS_WITH_YOUR_S3_BUCKET_NAME

sam deploy \
	--template-file packaged.yaml \
	--stack-name sns-to-websocket \
	--capabilities CAPABILITY_IAM \
	--parameter-overrides BroadcastTopic=REPLACE_THIS_WITH_YOUR_SNS_TOPIC_ARN

aws cloudformation describe-stacks 	\
	--stack-name sns-to-websocket --query 'Stacks[].Outputs'
```

## Demonstration

To test the WebSocket API, you can use [wscat](https://github.com/websockets/wscat), an open-source command line tool.

1. [Install NPM](https://www.npmjs.com/get-npm).
2. Install wscat:
``` bash
$ npm install -g wscat
```
3. The console output from the stack creation has an outputted value `WebSocketURI`. You can connect to that endpoint using:
``` bash
$ wscat -c wss://{YOUR-API-ID}.execute-api.{YOUR-REGION}.amazonaws.com/{STAGE}
```
4. In a separate console, send a notification to your SNS topic: 
``` bash
$ aws sns publish --topic-arn REPLACE_THIS_WITH_YOUR_SNS_TOPIC_ARN --message "Hello from an SNS topic"
```
You should see the SNS notification outputted to the console running `wscat`

## Disclaimer 
The generated API has no authentication or authorization. If you're working with non-public notifications, consider reading about how to add authorization to an API Gateway.


