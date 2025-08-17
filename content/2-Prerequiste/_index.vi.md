---
title : "Các bước chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

Phần này hướng dẫn bạn thiết lập **cơ sở hạ tầng ban đầu** cho pipeline ETL Serverless, bao gồm:

- **Hai bucket S3**:  
  - Một bucket để lưu trữ dữ liệu thô (CSV).  
  - Một bucket để lưu trữ dữ liệu đã xử lý (Parquet).  

- **IAM Roles**:  
  - Role cho AWS Glue (crawler + job).  
  - Role cho AWS Lambda (sẽ dùng ở bước sau để kích hoạt Glue job).  

Các bước này giúp bạn chuẩn bị hạ tầng cơ bản trước khi tạo Glue job và Lambda function.

---

### Các bước thực hiện

1. [Tạo S3 buckets](2.1-create-s3-buckets/)  
2. [Tạo IAM Roles cho Lambda và Glue](2.2-create-iam-roles/)  