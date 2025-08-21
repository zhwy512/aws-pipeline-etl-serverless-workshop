---
title : "Dọn dẹp tài nguyên"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---

Trong chương này, chúng ta sẽ **dọn dẹp toàn bộ tài nguyên** đã tạo trong bài lab để tránh phát sinh chi phí ngoài ý muốn.  

---

### 4.1. Xóa AWS Glue Resources

#### Xóa Glue Crawler
1. Truy cập **AWS Glue Console**.  
2. Chọn **Crawlers**.  
3. Chọn crawler đã tạo cho bài lab.  
4. Chọn **Actions → Delete crawler**.  
5. Xác nhận **Delete**.  

#### Xóa Glue Database
1. Trong **AWS Glue Console**, chọn **Databases**.  
2. Chọn database `event_db` (hoặc tên đã đặt).  
3. Chọn **Delete** và xác nhận.  

#### Xóa Glue Job
1. Vào **Jobs** trong Glue.  
2. Chọn `TransformRawDataJob`.  
3. Chọn **Delete job**.  

---

### 4.2. Xóa Lambda Function
1. Truy cập **AWS Lambda Console**.  
2. Chọn function `TriggerTransformJob`.  
3. Chọn **Actions → Delete** và xác nhận.  

> 💡 Bạn cũng có thể xóa **CloudWatch Log Group** của Lambda để tránh phát sinh chi phí log lưu trữ.  

---

### 4.3. Xóa S3 Buckets
1. Vào **S3 Console**.  
2. Chọn bucket `s3-raw-bucket-2025` → chọn **Empty**.  
   - Nhập `permanently delete` để xác nhận.  
3. Chọn lại bucket → **Delete bucket** → nhập chính xác tên bucket để xóa.  

Lặp lại các bước trên với bucket `s3-processed-bucket-2025`.  

---

### 4.4. Xóa IAM Roles
1. Truy cập **IAM Console**.  
2. Chọn **Roles**.  
3. Xóa các role đã tạo:  
   - `LambdaTriggerRole`  
   - `GlueServiceRole`
   - `GlueCrawlerRole`
4. Xác nhận xóa.  

---

### 4.5. Kiểm tra chi phí
- Truy cập **AWS Billing Console**.  
- Đảm bảo không còn tài nguyên nào đang hoạt động.  
