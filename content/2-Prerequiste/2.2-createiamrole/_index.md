---
title : "Create IAM Roles"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

### Create IAM Roles

In this step, we will create **3 IAM Roles** required for the pipeline:

1. **LambdaTriggerRole** – allows the Lambda function to start a Glue Job.  
2. **GlueServiceRole** – allows the Glue Job to read data from the raw S3 bucket, write processed data to the processed S3 bucket, and write logs to CloudWatch.  
3. **GlueCrawlerRole** – allows the Glue Crawler to read processed data from S3 and create tables in the Glue Data Catalog for Athena queries.  

---

### 1. Create IAM Role for Lambda (LambdaTriggerRole)

1. Go to the [IAM Management Console](https://console.aws.amazon.com/iamv2/).  
2. In the left navigation pane, select **Roles** → **Create role**.  
3. Choose **AWS service** → **Lambda** → click **Next**.  

   ![create-lambda-role](/images/2.prerequisite/iam-create-lambda-role.png)

4. In **Permissions policies**, search for and select the policy **AWSLambdaBasicExecutionRole**.  
5. Create an additional custom policy with the following content to allow Lambda to start a Glue Job:

  ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "glue:StartJobRun",
               "Resource": "*"
           }
       ]
   }
  ```

Attach this policy to the Role.

{{% notice tip %}}
In a production environment, you should replace `"Resource": "*"` with the specific ARN of the Glue Job to follow the **least privilege** principle.
{{% /notice %}}

6. Click **Next**, name the Role **LambdaTriggerRole**, then select **Create role**.

---

### 2. Create IAM Role for Glue (GlueServiceRole)

1. In the **Roles** page, click **Create role**.

2. Choose **AWS service** → **Glue** → click **Next**.

   !\[create-glue-role]\([https://chatgpt.com/images/2.prerequisite](https://chatgpt.com/images/2.prerequisite) iam-create-glue-role.png)

3. In **Permissions policies**, search for and select the built-in policy **AWSGlueServiceRole**.

4. Create and attach the following additional custom policies:

   * **Policy to allow Glue to read from the raw bucket:**

     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Effect": "Allow",
                 "Action": [
                     "s3:GetObject",
                     "s3:ListBucket"
                 ],
                 "Resource": [
                     "arn:aws:s3:::s3-raw-bucket-2025",
                     "arn:aws:s3:::s3-raw-bucket-2025/*"
                 ]
             }
         ]
     }
     ```

   * **Policy to allow Glue to write to the processed bucket:**

     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Effect": "Allow",
                 "Action": "s3:PutObject",
                 "Resource": "arn:aws:s3:::s3-processed-bucket-2025/*"
             }
         ]
     }
     ```

   * **Policy to allow Glue to write logs to CloudWatch:**

     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Effect": "Allow",
                 "Action": [
                     "logs:CreateLogGroup",
                     "logs:CreateLogStream",
                     "logs:PutLogEvents"
                 ],
                 "Resource": "arn:aws:logs:*:*:log-group:/aws-glue/jobs/logs-v2:*"
             }
         ]
     }
     ```

5. Click **Next**, name the Role **GlueServiceRole**, then select **Create role**.

---

### 3. Create IAM Role for Glue Crawler (GlueCrawlerRole)

1. In the **Roles** page, click **Create role**.

2. Choose **AWS service** → **Glue** → click **Next**.

3. In **Permissions policies**, search for and select the built-in policy **AWSGlueServiceRole**.

4. Create and attach the following consolidated custom policy:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "s3:GetBucketLocation",
                   "s3:GetBucketAcl",
                   "s3:ListAllMyBuckets",
                   "s3:GetObject",
                   "s3:ListBucket",
                   "s3:PutObject"
               ],
               "Resource": [
                   "arn:aws:s3:::s3-processed-bucket-2025",
                   "arn:aws:s3:::s3-processed-bucket-2025/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents"
               ],
               "Resource": "arn:aws:logs:*:*:*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "glue:StartCrawler",
                   "glue:GetCrawler",
                   "glue:UpdateCrawler",
                   "glue:CreateTable",
                   "glue:GetTable",
                   "glue:UpdateTable",
                   "glue:GetDatabase",
                   "glue:CreatePartition",
                   "glue:GetPartition"
               ],
               "Resource": [
                   "arn:aws:glue:us-east-1:206658986089:catalog",
                   "arn:aws:glue:us-east-1:206658986089:database/etl_workshop_db",
                   "arn:aws:glue:us-east-1:206658986089:table/etl_workshop_db/*"
               ]
           }
       ]
   }
   ```

5. Click **Next**, name the Role **GlueCrawlerRole**, then select **Create role**.

---

{{% notice note %}}
After creating these 3 roles, make sure to remember the **Role names** as you will need them in the Lambda and Glue Job configuration steps later.
{{% /notice %}}
