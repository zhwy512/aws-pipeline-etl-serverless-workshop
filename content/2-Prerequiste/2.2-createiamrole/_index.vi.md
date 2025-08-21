---
title : "Tạo IAM Role"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

#### Tạo IAM Role

Trong bước này, chúng ta sẽ tạo **IAM Policies** và **3 IAM Role** để pipeline hoạt động:

1. **LambdaTriggerRole** – cho phép Lambda function khởi chạy Glue Job.  
2. **GlueServiceRole** – cho phép Glue Job đọc dữ liệu từ bucket S3 thô, ghi dữ liệu đã xử lý sang bucket S3 processed và ghi log vào CloudWatch.  
3. **GlueCrawlerRole** – cho phép Glue Crawler đọc dữ liệu processed trong S3 và tạo bảng trong Glue Data Catalog để truy vấn bằng Athena.

---

### Bước 1: Tạo các IAM Policies

Trước tiên, chúng ta cần tạo các custom policies. Truy cập vào [IAM Management Console](https://console.aws.amazon.com/iamv2/) → **Policies** → **Create policy**.

#### 1.1. Policy cho Lambda khởi chạy Glue Job

**Tên Policy:** `LambdaStartGlueJobPolicy`

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
Trong môi trường production, bạn nên thay `"Resource": "*"` bằng ARN của Glue Job cụ thể để đảm bảo nguyên tắc **least privilege**.  
{{% /notice %}}

#### 1.2. Policy cho Glue đọc dữ liệu từ bucket raw

**Tên Policy:** `GlueReadRawBucketPolicy`

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

#### 1.3. Policy cho Glue ghi dữ liệu ra bucket processed

**Tên Policy:** `GlueWriteProcessedBucketPolicy`

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

#### 1.4. Policy cho Glue ghi log vào CloudWatch

**Tên Policy:** `GlueCloudWatchLogsPolicy`

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

#### 1.5. Policy cho Glue Crawler

**Tên Policy:** `GlueCrawlerComprehensivePolicy`

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

### Bước 2: Tạo các IAM Roles

#### 2.1. Tạo IAM Role cho Lambda (LambdaTriggerRole)

1. Trong [IAM Management Console](https://console.aws.amazon.com/iamv2/), chọn **Roles** → **Create role**.  
2. Chọn **AWS service** → **Lambda** → click **Next**.  
3. Trong phần **Permissions policies**, tìm và attach các policies sau:
   - **AWSLambdaBasicExecutionRole** (AWS managed policy)
   - **LambdaStartGlueJobPolicy** (custom policy đã tạo ở bước 1.1)

4. Click **Next**, đặt tên Role là **LambdaTriggerRole**, sau đó chọn **Create role**.

---

#### 2.2. Tạo IAM Role cho Glue (GlueServiceRole)

1. Trong giao diện **Roles**, click **Create role**.
2. Chọn **AWS service** → **Glue** → click **Next**.
3. Trong phần **Permissions policies**, tìm và attach các policies sau:
   - **AWSGlueServiceRole** (AWS managed policy)
   - **GlueReadRawBucketPolicy** (custom policy đã tạo ở bước 1.2)
   - **GlueWriteProcessedBucketPolicy** (custom policy đã tạo ở bước 1.3)
   - **GlueCloudWatchLogsPolicy** (custom policy đã tạo ở bước 1.4)

4. Click **Next**, đặt tên Role là **GlueServiceRole**, sau đó chọn **Create role**.

---

#### 2.3. Tạo IAM Role cho Glue Crawler (GlueCrawlerRole)

1. Trong giao diện **Roles**, click **Create role**.
2. Chọn **AWS service** → **Glue** → click **Next**.
3. Trong phần **Permissions policies**, tìm và attach các policies sau:
   - **AWSGlueServiceRole** (AWS managed policy)
   - **GlueCrawlerComprehensivePolicy** (custom policy đã tạo ở bước 1.5)

4. Click **Next**, đặt tên Role là **GlueCrawlerRole**, sau đó chọn **Create role**.
    
---

{{% notice note %}}  
Sau khi tạo xong các policies và 3 roles trên, bạn cần ghi nhớ **tên Role** để sử dụng ở các bước cấu hình Lambda và Glue Job tiếp theo.  
{{% /notice %}}