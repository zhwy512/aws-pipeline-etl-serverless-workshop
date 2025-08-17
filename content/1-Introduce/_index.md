---
title: "Introduction"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

## Overview

Before building the Serverless ETL pipeline, let’s explore the **Serverless model** and the role of AWS services in processing revenue data from community events **without server management**.

Workshop objectives:
- Automate the **Extract – Transform – Load (ETL)** process.
- Reduce infrastructure operation effort.
- Easily scale as data volume or number of events increases.

Practical example: When a CSV file containing registration and event revenue data is uploaded to S3, the system will:
1. AWS Lambda triggers the process.
2. AWS Glue transforms the data into Parquet.
3. Amazon Athena queries and analyzes the results.
4. Amazon CloudWatch logs and monitors.

---

## Serverless

Serverless is a cloud computing model where AWS fully manages the infrastructure, allowing you to focus on developing applications instead of maintaining systems.  

**You don’t need to worry about:**
- Server management and maintenance
- Security patching
- Manual scalability
- Infrastructure resources

**Serverless services in this pipeline:**
- **Compute:** AWS Lambda – automatically executes logic when new events occur.
- **Storage & processing:** Amazon S3 – data storage; AWS Glue – ETL execution.
- **Analytics:** Amazon Athena – querying processed data.
- **Monitoring:** Amazon CloudWatch – logging and monitoring.

---

## AWS Lambda

AWS Lambda allows running code without managing servers.

**Advantages:**
- Automatically scales with workload.
- Pay only when code runs.
- Supports multiple languages (Node.js, Python, ...).
- Natively integrates with S3, Glue, Athena.

**Role in the pipeline:**
- Receives trigger when a new CSV file is uploaded to S3.
- Performs initial data validation.
- Sends information to Glue for further processing.

---

## AWS Glue

AWS Glue is a Serverless ETL service that helps prepare data quickly.

**Key features:**
- Reads data from S3 and transforms it into Parquet.
- Automatically creates a Data Catalog.
- Integrates with Athena for querying.

**Role in the pipeline:**
- Acts as the "T" (Transform) stage of ETL.
- Standardizes and optimizes data for analysis.

---

## Workshop objectives

- Upload raw CSV files to an S3 bucket.
- Use Lambda to validate and check the data.
- Transform data into Parquet using AWS Glue.
- Save results into another S3 bucket.
- Query the data with Amazon Athena.
- Log and handle errors with CloudWatch.
