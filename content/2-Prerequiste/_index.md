---
title : "Preparation Steps"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

This section guides you through setting up the **initial infrastructure** for the Serverless ETL pipeline, including:

- **Two S3 buckets**:  
  - One bucket for storing raw data (CSV).  
  - One bucket for storing processed data (Parquet).  

- **IAM Roles**:  
  - A role for AWS Glue (crawler + job).  
  - A role for AWS Lambda (used later to trigger the Glue job).  

These steps prepare the basic infrastructure before creating Glue jobs and Lambda functions.

---

### Steps to follow

1. [Create S3 buckets](2.1-create-s3-buckets/)  
2. [Create IAM Roles for Lambda and Glue](2.2-create-iam-roles/)  