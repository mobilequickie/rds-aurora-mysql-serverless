# rds-aurora-mysql-serverless
Getting started with Amazon RDS Aurora MySQL Serverless with Data API for building cloud enabled serverless backends for mobile and web applications.

There are two easy steps to building this solution: Part 1. **Configure backend** by creating an Amazon Cognito Identity Pool, IAM Role(s), and adding permission to those roles for accessing Amazon Translate and Polly directly from a mobile app. Part 2. **Create a mobile app** to showcase natural language processing by cloning my sample app from GitHub and configuring it to use the values created in step #1.

## Create an Aurora MySQL Serverless Database
I created a CloudFormation template to automate the creation of an RDS Aurora MySQL Serverless Cluster, an AWS Secrets for storing the cluster master account credentials, an AWS Lambda function used to make CRUD operations directly against your serverless database, and all the permission needed.

1.	Click on the Launch Stack button to start provision your new Aurora MySQL Serverless Database.
    
    [![Launch Stack](https://s3-us-west-2.amazonaws.com/mobilequickie/speechtranslator/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=my-serverless-cluster&templateURL=https://s3.amazonaws.com/cloudformation-templates-useast-1/rds-aurora-serverless/rds-aurora-serverless.yml)

    This will launch the AWS CloudFormation Console, passing in the template, create a new stack, and automate the creation of an Aurora Serverless Cluster, database, and Lambda function.
2.	Click **Next** on the Select Template page
3.  Provide a Database name (e.g. MarketPlace) and a master username/password for connection to your serverless instance.
 ![Stack Output](https://s3.amazonaws.com/cloudformation-templates-useast-1/rds-aurora-serverless/stack-parmeters.png "CloudFormation Stack Parameters")
4.	Click **Next**
5.	On the Options page, leave all the defaults and click **Next**
6.	On the Review page, check the box to acknowledge that CloudFormation will create IAM resources and click **Create**.
7.	Wait for the *my-serverless-cluster* stack to reach a status of CREATE_COMPLETE
8.	With the my-serverless-cluster-stack selected, **click on the Outputs tab** and you should see three rows.
 ![Stack Output](https://s3.amazonaws.com/cloudformation-templates-useast-1/rds-aurora-serverless/cf-output.png "CloudFormation Stack Output of Cognito Identity Pool details")
9. Copy the Value for each of the three resources as we’ll be pasting those values into AWS CLI command for executing CRUD operations agains the serverless database.

## Enable Data API - Aurora Serverless Database
