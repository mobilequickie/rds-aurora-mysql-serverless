# Getting Started with Amazon RDS Aurora Serverless Data API
Getting started with Amazon RDS Aurora (MySQL) Serverless with Data API for building cloud enabled serverless backends for mobile and web applications.

If you want to learn a bit more about Aurora Server Data API, checkout my blog and video: 

<BLOG LINK HERE>
<YouTube Video Embed HERE>

The video will briefly go over Aurora Serverless and the new Data API but the real meat is about demonstrating the three (3) ways to deploy a new Aurora (MySQL) serverless database with Data API and exploring the four (4) ways to connect to your Aurora Serverless Data API enabled database.

If you don't care about the details or have already read/watched the solution, here's how you can get started quickly deploying your Aurora Cluster and enabling the Data API. In this solution below, we'll use a CloudFormation template that does the following:

[] Create a new RDS Aurora Serverless Cluster

[] Creates a blank database within the cluster
[] Creates an AWS Secrets to store the master user credentials for connecting to the database

[] Deploys a new AWS Lambda function with three environment variables (database name, cluster arn, and secrets arn) for connecting directly to your serverless database

[] Creates a Lambda execution role with permissions to the make CRUD operations against Aurora Serverless database, AWS Secrets for getting the master credentials, and logging to CloudWatch Logs.

Note: The CloudFormation template does not currently enable the "Data API" for the created Aurora Serverless Cluster as this feature is currently in BETA and the only way to enable Data API is through the RDS management console. See step 2 to enable the Data API manually after the Cloudformation Stack completes.

## Step 1: Create an Aurora Serverless Database via CloudFormation
This CloudFormation template automates the creation of an RDS Aurora MySQL Serverless Cluster, an AWS Secrets for storing the cluster master account credentials, an AWS Lambda function used to make CRUD operations directly against your serverless database, and all the permission needed.

Once the stack completes, see the Output tab for the Aurora Cluster Arn and Secrets Arn which will be used to for connecting to the database via the AWS CLI, SDK, or Lambda. Please note that these arn's are alos available as environment variables for the deployed Lambda function.

### GET STARTED

1. Click on the Launch Stack button to start provision your new Aurora MySQL Serverless Database.
    
    [![Launch Stack](https://s3-us-west-2.amazonaws.com/mobilequickie/speechtranslator/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=my-serverless-cluster&templateURL=https://s3.amazonaws.com/cloudformation-templates-useast-1/rds-aurora-serverless/rds-aurora-serverless.yml)

    This will launch the AWS CloudFormation Console, passing in the template, create a new stack, and automate the creation of an Aurora Serverless Cluster, database, and Lambda function.

2.	Click **Next** on the Select Template page
3.  Provide a Database name (e.g. MarketPlace) and a master username/password for connecting to your serverless instance.
 ![Stack Output](https://s3.amazonaws.com/cloudformation-templates-useast-1/rds-aurora-serverless/stack-parmeters.png "CloudFormation Stack Parameters")
4.	Click **Next**
5.	On the Options page, leave all the defaults and click **Next**
6.	On the Review page, check the box to acknowledge that CloudFormation will create IAM resources and click **Create**.
7.	Wait for the *my-serverless-cluster* stack to reach a status of CREATE_COMPLETE
8.	With the my-serverless-cluster-stack selected, **click on the Outputs tab** and you should see three rows.
 ![Stack Output](https://s3.amazonaws.com/cloudformation-templates-useast-1/rds-aurora-serverless/cf-output.png "CloudFormation Stack Output of Cognito Identity Pool details")
9. Copy the Value for each of the three resources as we’ll be pasting those values into AWS CLI command for executing CRUD operations agains the serverless database.

## Step 2: Enable Data API for your Aurora Serverless Cluster
Here we are going to enable the Data API for the newly created Aurora serverless cluster manually via the RDS Management Console. Hopefully this is temporary and we'll be able to enable via the CF template using EnableHTTPEndpointAPI.

1. Launch the [RDS Management Console](https://console.aws.amazon.com/rds/home?region=us-east-1#databases:)
2. Select your cluster (even though it says database)
3. Select the Modify button on the upper right
4. Select "Data API" under the Network & Security section
5. Click orange "Continue" button
6. Select "Apply immediately" under Scheduling of modifications
7. Select "Modify cluster" button

![](https://s3.amazonaws.com/cloudformation-templates-useast-1/rds-aurora-serverless/enable-data-api-animated.gif)

Congrats! The Aurora Serverless Cluster is now Data API enabled.


