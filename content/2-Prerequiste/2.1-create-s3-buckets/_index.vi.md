---
title : "Thiết lập S3 buckets"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

Trong bài lab này, chúng ta sẽ tạo **hai bucket S3** để lưu trữ dữ liệu:  
- Bucket **raw**: chứa file CSV thô.  
- Bucket **processed**: chứa dữ liệu đã xử lý (Parquet).

---

### Các bước thực hiện

1. Truy cập vào [AWS Management Console](https://aws.amazon.com/vi/console/).  
   - Tìm **S3** và chọn vào dịch vụ.  

   ![alt text](image-5.png)

2. Trong giao diện **S3**:
   - Chọn **Create bucket**.

   ![alt text](image.png)

   {{% notice note %}}
   Hãy đảm bảo bạn đang ở **đúng AWS Region** mà bạn dự định triển khai Glue và Lambda.
   {{% /notice %}}

3. Tạo bucket raw:
   - **Bucket name**: Nhập `s3-raw-bucket-2025`.

   {{% notice tip %}}  
   Tên bucket phải duy nhất toàn cầu. Nếu trùng, hãy thêm hậu tố như `-yourname`.  
   {{% /notice %}}

   ![alt text](image-6.png)

   - Bỏ chọn **Block all public access** và xác nhận.
  
   ![alt text](image-2.png)
   
   - Chọn **Create bucket**.

4. Chuẩn bị folder và upload dữ liệu:
   - Chọn bucket `s3-raw-bucket-2025`.

   ![alt text](image-3.png)

   - Chọn **Create folder** → nhập `etl-input` → **Create folder**.

   ![alt text](image-7.png)

   - Chọn folder `etl-input` → **Upload** → **Add files** → chọn file `event_data.csv`.
     > 📥 [Tải file mẫu tại đây](/files/event_data.csv)
   - Chọn **Upload** và đợi trạng thái **Succeeded**.

   ![alt text](image-8.png)

5. Tạo bucket processed tương tự:  
   - Quay lại danh sách bucket.  
   - Chọn **Create bucket**.  
   - **Bucket name**: `s3-processed-bucket-2025`.  
   - Bỏ chọn **Block all public access** → **Create bucket**.  

   ![alt text](image-9.png)

   {{% notice tip %}}  
   Không cần tạo thư mục hoặc upload file cho bucket processed — Glue sẽ tự tạo khi job chạy.  
   {{% /notice %}}