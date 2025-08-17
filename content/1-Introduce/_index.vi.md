---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

## Tổng quan

Trước khi bắt đầu xây dựng pipeline ETL Serverless, hãy tìm hiểu về **mô hình Serverless** và vai trò của các dịch vụ AWS trong việc xử lý dữ liệu doanh thu từ các sự kiện cộng đồng mà **không cần quản lý máy chủ**.

Mục tiêu của workshop:
- Tự động hóa quy trình **trích xuất – chuyển đổi – tải** (ETL) dữ liệu.
- Giảm công sức vận hành hạ tầng.
- Dễ dàng mở rộng khi dữ liệu hoặc số lượng sự kiện tăng.

Ví dụ thực tế: Khi một file CSV chứa thông tin đăng ký và doanh thu sự kiện được tải lên S3, hệ thống sẽ:
1. AWS Lambda kích hoạt xử lý.
2. AWS Glue chuyển đổi dữ liệu sang Parquet.
3. Amazon Athena truy vấn và phân tích kết quả.
4. Amazon CloudWatch ghi log và giám sát.

---

## Serverless

Serverless là mô hình điện toán đám mây nơi AWS quản lý hoàn toàn hạ tầng, cho phép bạn tập trung phát triển ứng dụng thay vì bảo trì hệ thống.  

**Bạn không cần lo về:**
- Quản lý và bảo trì máy chủ
- Cập nhật bản vá bảo mật
- Khả năng mở rộng thủ công
- Tài nguyên hệ thống

**Các dịch vụ Serverless trong pipeline này:**
- **Điện toán:** AWS Lambda – xử lý logic tự động khi có sự kiện mới.
- **Lưu trữ & xử lý:** Amazon S3 – lưu trữ dữ liệu; AWS Glue – thực hiện ETL.
- **Phân tích:** Amazon Athena – truy vấn dữ liệu đã xử lý.
- **Giám sát:** Amazon CloudWatch – theo dõi và log.

---

## AWS Lambda

AWS Lambda cho phép chạy code mà không cần quản lý máy chủ.

**Ưu điểm:**
- Tự động mở rộng theo tải.
- Chỉ tính phí khi code thực thi.
- Hỗ trợ nhiều ngôn ngữ (Node.js, Python, ...).
- Tích hợp chặt chẽ với S3, Glue, Athena.

**Vai trò trong pipeline:**
- Nhận trigger khi có file CSV mới trên S3.
- Thực hiện xác thực dữ liệu ban đầu.
- Gửi thông tin sang Glue để xử lý tiếp.

---

## AWS Glue

AWS Glue là dịch vụ ETL Serverless giúp chuẩn bị dữ liệu nhanh chóng.

**Tính năng chính:**
- Đọc dữ liệu từ S3, chuyển đổi sang Parquet.
- Tự động tạo Data Catalog.
- Tích hợp với Athena để truy vấn.

**Vai trò trong pipeline:**
- Là công đoạn "T" (Transform) của ETL.
- Chuẩn hóa và tối ưu dữ liệu cho phân tích.

---

## Mục tiêu workshop

- Tải file CSV thô lên S3 bucket.
- Dùng Lambda để kiểm tra và xác thực dữ liệu.
- Chuyển đổi dữ liệu sang Parquet với AWS Glue.
- Lưu kết quả vào S3 bucket khác.
- Truy vấn dữ liệu với Amazon Athena.
- Ghi log và xử lý lỗi qua CloudWatch.