---
title : "Clean Up Resources"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---

In this chapter, we will **clean up all resources** created during the lab to avoid incurring unexpected charges.  

---

### 4.1. Delete AWS Glue Resources

#### Delete Glue Crawler
1. Go to the **AWS Glue Console**.  
2. Select **Crawlers**.  
3. Choose the crawler created for this lab.  
4. Click **Actions → Delete crawler**.  
5. Confirm **Delete**.  

![Delete crawler](/images/4-cleanup/delete-crawler.png)

#### Delete Glue Database
1. In the **AWS Glue Console**, select **Databases**.  
2. Choose the database `event_db` (or the name you assigned).  
3. Click **Delete** and confirm.  

![Delete database](/images/4-cleanup/delete-database.png)

#### Delete Glue Job
1. Open **Jobs** in Glue.  
2. Select `TransformRawDataJob`.  
3. Click **Delete job**.  

![Delete job](/images/4-cleanup/delete-job.png)

---

### 4.2. Delete Lambda Function
1. Go to the **AWS Lambda Console**.  
2. Select the function `TriggerTransformJob`.  
3. Click **Actions → Delete** and confirm.  

![Delete lambda](/images/4-cleanup/delete-lambda.png)

> 💡 You may also delete the **CloudWatch Log Group** for the Lambda function to avoid log storage costs.  

---

### 4.3. Delete S3 Buckets
1. Open the **S3 Console**.  
2. Select the bucket `s3-raw-bucket-2025` → click **Empty**.  
   - Enter `permanently delete` to confirm.  
3. Select the bucket again → **Delete bucket** → type the exact bucket name to confirm deletion.  

Repeat the same steps for the bucket `s3-processed-bucket-2025`.  

![Delete bucket](/images/4-cleanup/delete-bucket.png)

---

### 4.4. Delete IAM Roles
1. Go to the **IAM Console**.  
2. Select **Roles**.  
3. Delete the roles created for this lab:  
   - `LambdaTriggerRole`  
   - `GlueServiceRole`  
   - `GlueCrawlerRole`  
4. Confirm deletion.  

![Delete IAM role](/images/4-cleanup/delete-role.png)

---

### 4.5. Verify Billing
- Open the **AWS Billing Console**.  
- Ensure no active resources remain.  

![Billing dashboard](/images/4-cleanup/billing.png)
