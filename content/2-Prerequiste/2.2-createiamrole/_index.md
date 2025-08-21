---
title : "Creating IAM Roles"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

#### Creating IAM Roles

In this step, we will create **IAM Policies** and **3 IAM Roles** for the pipeline to function:

1. **LambdaTriggerRole** – allows Lambda function to start Glue Jobs.  
2. **GlueServiceRole** – allows Glue Job to read data from raw S3 bucket, write processed data to processed S3 bucket, and write logs to CloudWatch.  
3. **GlueCrawlerRole** – allows Glue Crawler to read processed data in S3 and create tables in Glue Data Catalog for querying with Athena.

---

### Step 1: Create IAM Policies

First, we need to create custom policies. Go to [IAM Management Console](https://console.aws.amazon.com/iamv2/) → **Policies** → **Create policy**.

#### 1.1. Policy for Lambda to Start Glue Job

**Policy Name:** `LambdaStartGlueJobPolicy`

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

{{% notice tip %}}  
In production environments, you should replace `"Resource": "*"` with the specific Glue Job ARN to follow the **least privilege** principle.  
{{% /notice %}}

#### 1.2. Policy for Glue to Read Data from Raw Bucket

**Policy Name:** `GlueReadRawBucketPolicy`

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

#### 1.3. Policy for Glue to Write Data to Processed Bucket

**Policy Name:** `GlueWriteProcessedBucketPolicy`

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

#### 1.4. Policy for Glue to Write Logs to CloudWatch

**Policy Name:** `GlueCloudWatchLogsPolicy`

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

#### 1.5. Policy for Glue Crawler

**Policy Name:** `GlueCrawlerComprehensivePolicy`

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

---

### Step 2: Create IAM Roles

#### 2.1. Create IAM Role for Lambda (LambdaTriggerRole)

1. In the [IAM Management Console](https://console.aws.amazon.com/iamv2/), select **Roles** → **Create role**.  
2. Choose **AWS service** → **Lambda** → click **Next**.  
3. In the **Permissions policies** section, find and attach the following policies:
   - **AWSLambdaBasicExecutionRole** (AWS managed policy)
   - **LambdaStartGlueJobPolicy** (custom policy created in step 1.1)

4. Click **Next**, name the Role **LambdaTriggerRole**, then select **Create role**.

---

#### 2.2. Create IAM Role for Glue (GlueServiceRole)

1. In the **Roles** interface, click **Create role**.
2. Choose **AWS service** → **Glue** → click **Next**.
3. In the **Permissions policies** section, find and attach the following policies:
   - **AWSGlueServiceRole** (AWS managed policy)
   - **GlueReadRawBucketPolicy** (custom policy created in step 1.2)
   - **GlueWriteProcessedBucketPolicy** (custom policy created in step 1.3)
   - **GlueCloudWatchLogsPolicy** (custom policy created in step 1.4)

4. Click **Next**, name the Role **GlueServiceRole**, then select **Create role**.

---

#### 2.3. Create IAM Role for Glue Crawler (GlueCrawlerRole)

1. In the **Roles** interface, click **Create role**.
2. Choose **AWS service** → **Glue** → click **Next**.
3. In the **Permissions policies** section, find and attach the following policies:
   - **AWSGlueServiceRole** (AWS managed policy)
   - **GlueCrawlerComprehensivePolicy** (custom policy created in step 1.5)

4. Click **Next**, name the Role **GlueCrawlerRole**, then select **Create role**.
    
---

{{% notice note %}}  
After creating the policies and 3 roles above, make sure to remember the **Role names** for use in the subsequent Lambda and Glue Job configuration steps.  
{{% /notice %}}