---
title: "Blog 2"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Truyền dữ liệu bảng Amazon DynamoDB sang Amazon S3 Tables để phân tích


Các tổ chức sử dụng Amazon DynamoDB cho các khối lượng công việc giao dịch thường cần thực hiện phân tích dữ liệu phức tạp. Mặc dù DynamoDB rất xuất sắc trong việc xử lý khối lượng giao dịch lớn, nhiều tổ chức vẫn duy trì kho dữ liệu phân tích riêng để chạy các truy vấn phức tạp và tạo insight mà không ảnh hưởng đến hiệu năng của cơ sở dữ liệu giao dịch.

Trong bài viết này, chúng tôi trình bày cách truyền dữ liệu từ DynamoDB đến Amazon S3 Tables để khai thác khả năng phân tích mạnh mẽ trên dữ liệu vận hành. Mẫu luồng này lý tưởng cho các tổ chức cần kiểm soát chặt chẽ hơn đối với việc chuyển đổi dữ liệu trong môi trường nhạy cảm.

Giải pháp đặc biệt có giá trị cho các nhóm xây dựng data lake, muốn một giải pháp quản lý tốt với định dạng Apache Iceberg và tích hợp cùng các dịch vụ phân tích của AWS mà không làm tăng gánh nặng vận hành.

## Lưu trữ dữ liệu dạng bảng trong Amazon S3

S3 Tables, được công bố tại AWS reInvent 2024, là dịch vụ lưu trữ đối tượng đám mây đầu tiên hỗ trợ Iceberg nguyên bản, thiết kế để đơn giản hóa việc lưu trữ dữ liệu dạng bảng ở quy mô lớn. Dữ liệu dạng bảng có cấu trúc cột và hàng, giống như bảng trong cơ sở dữ liệu. Dữ liệu trong S3 Tables được lưu trong một loại bucket mới, gọi là table bucket, chứa các bảng như tài nguyên Amazon S3.

Table buckets hỗ trợ lưu trữ theo định dạng Iceberg và có thể truy vấn bằng SQL thông qua công cụ như Amazon Athena, Apache Spark, và Amazon Redshift. Bạn cũng có thể tích hợp bucket bảng với AWS Glue Data Catalog để khám phá và truy cập dữ liệu.

Tích hợp này cho phép bạn làm việc với dữ liệu qua Athena, Amazon Redshift và Amazon QuickSight. Tham khảo thêm tại Using Amazon S3 Tables with AWS analytics services.

S3 Tables cung cấp khả năng quản lý bảng tích hợp: theo dõi metadata, tiến hóa schema, và tự động tổ chức dữ liệu. Continuous table optimization quét và viết lại dữ liệu nền, giúp truy vấn nhanh hơn tới 3 lần so với bảng Iceberg không được quản lý.

Ngoài ra, S3 Tables có tối ưu hóa riêng cho workload Iceberg, hỗ trợ số giao dịch mỗi giây gấp 10 lần so với các bảng Iceberg lưu trong bucket S3 thường. Một số lợi ích chính:

Giao dịch ACID và truy vấn time-travel

Hỗ trợ tiến hóa schema

Hiệu suất cao nhờ partitioning và columnar storage

Tích hợp nguyên bản với Apache Spark

Bảo trì liên tục: compaction, snapshot management, dọn dẹp file

### Tổng quan giải pháp

Change data capture (CDC) là quá trình ghi lại thay đổi từ cơ sở dữ liệu và phát vào luồng sự kiện. Amazon DynamoDB change data capture cung cấp cơ chế ghi nhận, xử lý, và phản ứng với các thay đổi gần như theo thời gian thực.

DynamoDB hỗ trợ 2 mô hình streaming cho CDC: Amazon DynamoDB Streams và Amazon Kinesis Data Streams for DynamoDB. Bài viết minh họa 2 cách triển khai để truyền dữ liệu từ DynamoDB sang S3 Tables.

### Mẫu 1: Sử dụng DynamoDB Streams (khuyên dùng cho hầu hết trường hợp)

DynamoDB Streams ghi nhận thay đổi và truyền qua AWS Lambda xử lý, sau đó chuyển tiếp sự kiện đến Amazon Data Firehose để ghi vào S3 Tables. Streams giữ dữ liệu trong 24 giờ, phù hợp khi cần replication gần real-time mà không cần hạ tầng phức tạp.

![](/images/3-BlogsTranslated/3.2-Blog2/image-1-1.png)

Amazon SQS được dùng làm dead-letter queue (DLQ) lưu các sự kiện lỗi để retry bằng Lambda với back-off strategy.

### Mẫu 2: Sử dụng Amazon Kinesis Data Streams

Mẫu này phù hợp khi cần retention dài hơn (tới 365 ngày) hoặc hỗ trợ nhiều consumer fan-out. Kinesis Data Streams tích hợp tốt với các dịch vụ AWS thông qua Firehose.

![](/images/3-BlogsTranslated/3.2-Blog2/image-2-1.png)

Việc xử lý lỗi có thể thực hiện ở nhiều tầng:

Ở Kinesis: cấu hình replay và giám sát qua Amazon CloudWatch alarms

Ở Lambda: dùng DLQ, structured logging, exactly-once processing

Ở Firehose: backup bản ghi lỗi vào S3, retry có cấu hình, logging CloudWatch

Cách tiếp cận này giúp đảm bảo độ tin cậy và khả năng phục hồi dữ liệu.

### Các thành phần chính của kiến trúc

Athena – dịch vụ truy vấn SQL serverless

Amazon Data Firehose – xử lý delivery đến S3 Tables

DynamoDB – nguồn dữ liệu giao dịch

DynamoDB Streams hoặc Kinesis Data Streams – ghi nhận thay đổi

AWS Lake Formation – quản trị truy cập dữ liệu

Lambda – xử lý record từ stream

S3 Tables – lưu trữ dữ liệu phân tích theo Iceberg

Tham khảo thêm: Streaming options for change data capture.

Bucket bảng S3 có thể tích hợp với Amazon SageMaker Lakehouse và các dịch vụ phân tích như Athena, Redshift, EMR, QuickSight.

Trong môi trường production, nên phân tầng data lake (raw, cleansed, curated). Tầng raw có thể lưu Parquet, còn tầng curated dùng Iceberg để tận dụng time travel, schema evolution, và ACID.

## Điều kiện tiên quyết

## Clone repository

CODE

## Cập nhật context cho AWS CDK

CODE

stream_type có thể là:

kinesis – dùng Kinesis Data Streams

dynamodb – dùng DynamoDB Streams

## Triển khai tài nguyên pipeline

CODE

## Triển khai quyền trong Lake Formation

Stack này thiết lập metadata integration, quyền quản trị, cross-catalog access, IAM roles.

CODE

## Triển khai Firehose delivery stream

Stack tạo delivery stream tới S3 Tables, cấu hình buffer, error handling, logging CloudWatch.

CODE

## Truyền dữ liệu từ DynamoDB sang S3 Tables

CODE

## Chạy phân tích trên S3 Tables

Bạn có thể truy vấn ngay bằng Amazon Athena. Ngoài ra, cũng có thể kết nối engine khác như Trino, Presto, Spark.

### Dùng Athena CLI

CODE
CODE

### Dùng Athena Console

IMAGE

Ví dụ truy vấn:

CODE
CODE

## Cấu hình bảo trì S3 Tables

CODE

## Dọn dẹp tài nguyên

CODE

## Kết luận

Giải pháp cho thấy cách tích hợp DynamoDB với S3 Tables để phân tích qua CDC. Kiến trúc serverless giúp giảm vận hành, đồng thời tận dụng Iceberg cho workloads phân tích.

Ngoài ra, bạn có thể cân nhắc các dịch vụ như Amazon Redshift zero-ETL hoặc Amazon OpenSearch Service cho các nhu cầu phân tích khác.