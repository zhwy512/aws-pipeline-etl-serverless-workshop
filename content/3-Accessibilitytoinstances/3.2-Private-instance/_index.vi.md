---
title : "Cấu hình Lambda trigger"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

Trong bước này chúng ta sẽ tạo một Lambda function `TriggerTransformJob` để **tự động kích hoạt Glue Job** `TransformRawDataJob` mỗi khi có file CSV mới được upload vào `s3://s3-raw-bucket-2025/etl-input/`.

---

### 1. Tải file Lambda mẫu:

👉 [Nhấn vào đây để tải file .zip](/files/lambda-trigger-transform.zip)

File này chứa sẵn mã nguồn với logic:
- Nhận event từ S3 (bucket + object key).  
- Gọi `glue.startJobRun` với tham số `--input_path` là đường dẫn file CSV vừa upload.  
- In ra JobRunId của Glue job để theo dõi.  

---

### 2. Tạo Lambda Function

1. Mở [AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1).  
2. Chọn **Create function** → **Author from scratch**:
   - **Function name**: `TriggerTransformJob`  
   - **Runtime**: `Node.js 22.x`
   
   ![alt text](image.png)
   
   - **Execution role**: mở rộng phần **Change default execution role** → chọn **Use an existing role** → `LambdaTriggerRole` (tạo ở bước 2.2).  
   
   ![alt text](image-1.png)
   
   - Bấm **Create function**.  

3. Trong tab **Code**, chọn **Upload from → .zip file** → chọn file `.zip` vừa tải về → click **Deploy**. 

---

### 3. Thêm Trigger S3

1. Vào tab **Configuration → Triggers → Add trigger**.  
2. Chọn **S3**:  
   - **Bucket**: `s3-raw-bucket-2025`  
   - **Event type**: `All object create events`  
   - **Prefix**: `etl-input/`  
   - **Suffix**: `.csv` (để chỉ bắt file CSV, tuỳ chọn)  
   - Tích vào ô xác nhận chi phí → **Add**. 
   
   ![alt text](image-2.png)

---

### 4. Kiểm tra Lambda bằng Test Event

Bạn có thể test trực tiếp trong Lambda console bằng cách tạo một **Test event** với nội dung sau:

```json
{
  "Records": [
    {
      "s3": {
        "bucket": {
          "name": "s3-raw-bucket-2025"
        },
        "object": {
          "key": "etl-input/event_data.csv"
        }
      }
    }
  ]
}
```

Khi chạy test, Lambda sẽ in ra JobRunId mới của Glue job `TransformRawDataJob`.

---

### 5. Xác minh toàn bộ pipeline

1. Upload file CSV thật (vd: `etl-input/sales.csv`) lên `s3://s3-raw-bucket-2025/etl-input/`.
    
2. Lambda tự động được kích hoạt và gọi Glue job.
    
3. Vào **Glue → Jobs → TransformRawDataJob** để xem run mới.
    
4. Sau khi Glue job chạy xong, kiểm tra kết quả trong `s3://s3-processed-bucket-2025/transformed/`.
    

---

### 6. Lưu ý

- Nếu Lambda không được trigger, kiểm tra lại **S3 Event Notifications** và IAM permission của Lambda.  
- Nếu Lambda bị lỗi khi gọi Glue, kiểm tra policy `glue:StartJobRun` trong role `LambdaTriggerRole`.
- Lambda **chỉ nên gọi start job** và thoát; việc xử lý chính được Glue đảm nhiệm.