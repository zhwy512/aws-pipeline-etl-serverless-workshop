---
title : "Triển khai và kiểm tra pipeline"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

Phần này sẽ hướng dẫn bạn triển khai pipeline ETL Serverless bằng cách tạo và cấu hình AWS Glue job để chuyển đổi dữ liệu từ bucket S3 thô sang bucket đã xử lý, đồng thời kiểm tra quy trình hoạt động. Bạn sẽ sử dụng các thành phần đã thiết lập (S3 buckets, Lambda trigger) và đảm bảo dữ liệu được xử lý đúng định dạng Parquet.

### Các bước thực hiện
3.1. [Xây dựng Glue Job và kiểm tra dữ liệu](3.1-public-instance/) \
3.2. [Cấu hình Lambda trigger](3.2-private-instance/) \
3.3. [Truy vấn dữ liệu với Athena](3.3-/) 