# Serverless ETL Pipeline with AWS Glue and Lambda

## 📌 Project Description
This project demonstrates the development of a **serverless ETL (Extract, Transform, Load) pipeline** using **AWS Glue** and **AWS Lambda**.  
The pipeline ingests raw data from Amazon S3, processes and validates it with Glue jobs, applies transformations, and stores the cleaned data back into S3.  
The processed data can then be queried using **Amazon Athena** for analytics and reporting.  

The solution focuses on:
- **ETL optimization**
- **Data validation**
- **Error handling**
- **Monitoring setup**
- **Cost optimization**
- **Operational procedures**
- **Data quality assurance**
- **Testing framework**

---

## 🏗️ Architecture Overview
1. **Data Source (S3)**: Raw data stored in an S3 bucket.  
2. **AWS Lambda (Trigger + Orchestration)**: Invoked by S3 events to trigger Glue ETL jobs.  
3. **AWS Glue (ETL Transformation)**: Cleans, validates, and transforms raw data.  
4. **Processed Data (S3)**: Output data written to a separate S3 bucket.  
5. **Amazon Athena (Query Engine)**: Enables querying of the processed dataset using SQL.  
6. **CloudWatch & Monitoring**: Logs, metrics, and alerts for monitoring and troubleshooting.  

---

## 🚀 Features
- Fully **serverless** architecture (no servers to manage).  
- Automatic ETL pipeline triggered by **S3 events**.  
- Supports **data validation and error handling**.  
- Provides **monitoring and logging** for debugging.  
- **Cost-efficient** by using on-demand AWS services.  
- Query-ready data available through **Athena**.  

---

## ⚙️ Requirements
- AWS Account with access to:
  - S3
  - Lambda
  - Glue
  - Athena
  - CloudWatch
- AWS CLI configured  
- Node.js 22.x

---

## 🔧 Setup & Deployment
1. Upload Lambda code to AWS Lambda.
2. Upload Glue job scripts to AWS Glue.
3. Configure S3 event triggers to invoke Lambda.
4. Test the pipeline by uploading raw data into the source S3 bucket.

---

## 📊 Usage

* Upload a raw dataset (CSV/JSON/Parquet) to the **raw S3 bucket**.
* Lambda triggers the Glue job for ETL processing.
* The processed dataset is stored in the **processed S3 bucket**.
* Run SQL queries on the data via **Athena**.

---

## 🛠️ Monitoring

* **CloudWatch Logs**: For Lambda and Glue job logs.
* **CloudWatch Metrics**: For monitoring job success/failure.
* **Alerts**: Can be configured using CloudWatch Alarms and SNS.
Bạn có muốn mình viết thêm **Hướng dẫn chi tiết (step-by-step)** trong README luôn không, hay chỉ để bản khung này còn docs chi tiết thì để trong thư mục `docs/`?
```
