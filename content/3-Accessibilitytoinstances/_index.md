---
title : "Deploying and Testing the Pipeline"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

This section will guide you through deploying the Serverless ETL pipeline by creating and configuring an AWS Glue job to transform data from the raw S3 bucket into the processed bucket, while also testing the workflow. You will use the components already set up (S3 buckets, Lambda trigger) and ensure that the data is correctly processed in Parquet format.

### Steps
3.1. [Build the Glue Job and Validate the Data](3.1-public-instance/) \
3.2. [Configure the Lambda Trigger](3.2-private-instance/) \
3.3. [Query the Data with Athena](3.3-/)  
