---
title : "Tạo IAM Role"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

### Tạo IAM Role

Trong bước này, chúng ta sẽ tạo **3 IAM Role** để pipeline hoạt động:

1. **LambdaTriggerRole** – cho phép Lambda function khởi chạy Glue Job.  
2. **GlueServiceRole** – cho phép Glue Job đọc dữ liệu từ bucket S3 thô, ghi dữ liệu đã xử lý sang bucket S3 processed và ghi log vào CloudWatch.  
3. **GlueCrawlerRole** – cho phép Glue Crawler đọc dữ liệu processed trong S3 và tạo bảng trong Glue Data Catalog để truy vấn bằng Athena.

---

### 1. Tạo IAM Role cho Lambda (LambdaTriggerRole)

1. Truy cập vào [IAM Management Console](https://console.aws.amazon.com/iamv2/).  
2. Trong thanh điều hướng bên trái, chọn **Roles** → **Create role**.  
3. Chọn **AWS service** → **Lambda** → click **Next**.  

   ![create-lambda-role](/images/2.prerequisite/iam-create-lambda-role.png)

4. Trong phần **Permissions policies**, tìm và chọn policy **AWSLambdaBasicExecutionRole**.  
5. Tạo thêm một policy tùy chỉnh có nội dung sau để cho phép Lambda khởi chạy Glue Job:

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
Gán policy này cho Role.

{{% notice tip %}}  
Trong môi trường production, bạn nên thay `"Resource": "*"` bằng ARN của Glue Job cụ thể để đảm bảo nguyên tắc **least privilege**.  
{{% /notice %}}

6. Click **Next**, đặt tên Role là **LambdaTriggerRole**, sau đó chọn **Create role**.

---

### 2. Tạo IAM Role cho Glue (GlueServiceRole)

1. Trong giao diện **Roles**, click **Create role**.
2. Chọn **AWS service** → **Glue** → click **Next**.

   ![create-glue-role](https://chatgpt.com/images/2.prerequisite iam-create-glue-role.png)

3. Trong phần **Permissions policies**, tìm và chọn policy có sẵn **AWSGlueServiceRole**.
4. Tạo và gán thêm các policy tùy chỉnh sau:
  - **Policy cho phép Glue đọc dữ liệu từ bucket raw:**
    
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

  - **Policy cho phép Glue ghi dữ liệu ra bucket processed:**
    
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

  - **Policy cho phép Glue ghi log vào CloudWatch:**

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

5. Click **Next**, đặt tên Role là **GlueServiceRole**, sau đó chọn **Create role**.

---

### 3. Tạo IAM Role cho Glue Crawler (GlueCrawlerRole)

1. Trong giao diện **Roles**, click **Create role**.
2. Chọn **AWS service** → **Glue** → click **Next**.
3. Trong phần **Permissions policies**, tìm và chọn policy có sẵn **AWSGlueServiceRole**.
4. Tạo và gán thêm một policy tùy chỉnh hợp nhất:
    
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
    
5. Click **Next**, đặt tên Role là **GlueCrawlerRole**, sau đó chọn **Create role**.
    
---

{{% notice note %}}  
Sau khi tạo xong 3 role trên, bạn cần ghi nhớ **tên Role** để sử dụng ở các bước cấu hình Lambda và Glue Job tiếp theo.  
{{% /notice %}}