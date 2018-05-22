# Serverless Slack News App

This is a Serverless application to post the top 5 news articles to a chosen slack channel. 

## Pre-requisites 

For setting up this application, you will need an AWS account and a slack workspace

## Deployment

There are multiples ways to deploy this application,below are command line instruction steps.  

* Create the stuff and upload it....
```
claudia create --region <YOUR_REGION> --handler index.handler --timeout 10 --set-env-from-json env.json
```
* Set the event and time
```
aws events put-rule --name hackerNewsDigest --schedule-expression 'cron(0 9 * * ? *)' --profile claudia
```
* You will get a similar response
```
{
     "RuleArn": "arn:aws:events:<YOUR_REGION>:<YOUR_ACCOUNT>:rule/hackerNewsDigest"
}
```
* Add permission for event to invoke lambda function
```
aws lambda add-permission --statement-id 'hackernews-scheduled-messages' --action 'lambda:InvokeFunction' --principal 'events.amazonaws.com' --source-arn <YOUR_EVENT_RULE_ARN> --function-name serverless-slack-news-app --region <YOUR_REGION> --profile claudia
```
* NOTE: you can set a profile in .aws credentials and run in a certain account
```
claudia create — region <YOUR_REGION> — handler index.handler — timeout 10 — set-env-from-json env.json --profile claudia
```
* NOTE: remember your lambda ARN
```
Remember <YOUR_LAMBDA_ARN>
```
* Trigger your lambda now by running the following
```
aws events put-targets --rule hackerNewsDigest --targets '[{ "Id": "1", "Arn": "<YOUR_LAMBDA_ARN>" }]' --profile claudia
```
* NOTE: to test the slack URL
```
curl -X POST -H 'Content-type: application/json' --data '{"text":"Allow me to re-invoke myself!"}' <YOUR_SLACK_WEBPHOCK_URL>

```

## Settings

The app can be set to run a certain times using a CRON job. 

```
cron tine example = 0 9 * * ? *
```

> **NB.** The above example equates to 10am every day

## Additional configuration

In order for the project to work, you will also need to create an env.json file with the slack web hock url.

```
{
  "SlackWebhookUrl": "<YOUR_SLACK_WEB_HOCK_URL_HERE>"
}
```

## Reference

[Scheduling Slack messages using AWS Lambda](https://medium.freecodecamp.org/scheduling-slack-messages-using-aws-lambda-e56a8eb22818) \
[INSTALLING AND CONFIGURING CLAUDIA.JS](https://claudiajs.com/tutorials/installing.html) \
[Slack Incoming Webhooks](https://api.slack.com/incoming-webhooks)