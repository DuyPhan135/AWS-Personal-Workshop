---
title: "Worklog Tuần 5"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---


### Mục tiêu tuần 5:

* Nắm bắt và hiểu các công nghệ Route53, CLI, DynamoDB, ElasstiCache (Redis), Cloud Front

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                              | Ngày bắt đầu  | Ngày hoàn thành  | Nguồn tài liệu                            | Ghi chú			   												     |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ---------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 2   | - Tạo **public hosted zone** và thêm **record** để trỏ tên miền đến địa chỉ IP                                                                                                  		       | 06/10/2025    | 06/10/2025       | <https://000010.awsstudygroup.com/vi/>    | Tìm hiểu cách Route 53, một dịch vụ DNS có khả năng mở rộng, ánh xạ tên miền tới địa chỉ IP.		 		     | 
| 3   | - Cài đặt và cấu hình **AWS CLI**. Thực hành các lệnh cơ bản để liệt kê **S3 buckets** hoặc mô tả **EC2 instances**                                              				       | 07/10/2025    | 07/10/2025       | <https://000011.awsstudygroup.com/vi/>    | Hiểu rằng AWS CLI là công cụ quản lý dịch vụ AWS qua dòng lệnh, giúp tự động hóa thao tác quản trị.			     |
| 4   | - Tạo một bảng **DynamoDB** đơn giản. Sử dụng giao diện console để thêm, cập nhật và xóa dữ liệu 												       | 08/10/2025    | 08/10/2025       | <https://000060.awsstudygroup.com/vi/>    | Tìm hiểu DynamoDB là cơ sở dữ liệu NoSQL được quản lý và cách thực hiện các thao tác CRUD cơ bản.		             |
| 5   | - Khởi chạy **Redis cluster**. Kết nối và thực hành các lệnh cơ bản **SET** và **GET**                            										       | 09/10/2025    | 09/10/2025       | <https://000061.awsstudygroup.com/vi/>    | Hiểu cách ElastiCache (Redis) hoạt động như một dịch vụ bộ nhớ đệm trong RAM giúp tăng hiệu năng ứng dụng.		     | 
| 6   | - Tạo **CloudFront distribution** cho **S3 bucket** chứa trang web tĩnh                                                                                     				               | 10/10/2025    | 10/10/2025       | <https://000094.awsstudygroup.com/vi/>    | Tìm hiểu CloudFront là mạng phân phối nội dung (CDN) giúp tăng tốc độ tải bằng cách lưu cache tại các điểm biên (edge).	     |


### Kết quả đạt được tuần 5:
* Nắm vững kiến thức thực hành về các dịch vụ cốt lõi của AWS thuộc nhiều nhóm khác nhau:
  * Networking & Content Delivery: Route 53 (DNS), CloudFront (CDN)
  * Database & Storage: DynamoDB (Cơ sở dữ liệu NoSQL), ElastiCache for Redis (Bộ nhớ đệm trong RAM)
  * Management Tools: AWS CLI (Giao diện dòng lệnh)

* Cấu hình quản lý DNS bằng Amazon Route 53
  * Tạo public hosted zone và thêm A record ánh xạ tên miền với địa chỉ IP công khai
  * Hiểu cách Route 53 cung cấp dịch vụ DNS mở rộng, giúp định tuyến tên miền đến tài nguyên web hiệu quả và tin cậy
  * Cài đặt và cấu hình AWS CLI để quản lý tài nguyên qua dòng lệnh

* Thiết lập Access Key, Secret Key, và Default Region
  * Thực hành các lệnh như liệt kê S3 buckets, mô tả EC2 instances, kiểm tra thông tin cấu hình
  * Hiểu rõ cách AWS CLI hỗ trợ tự động hóa và quản lý tài nguyên AWS linh hoạt

* Xây dựng bảng DynamoDB để tìm hiểu về cơ sở dữ liệu NoSQL
  * Tạo bảng thông qua giao diện AWS Management Console
  * Thực hành các thao tác CRUD (Tạo, Đọc, Cập nhật, Xóa)
  * Hiểu rõ bản chất serverless của DynamoDB trong việc xử lý dữ liệu có cấu trúc ở quy mô lớn

* Triển khai cụm ElastiCache (Redis) để tối ưu hiệu năng ứng dụng
  * Khởi tạo và kết nối đến Redis cluster
  * Thực hành các lệnh SET và GET để lưu và truy xuất dữ liệu trong bộ nhớ đệm
  * Nắm được vai trò của in-memory caching trong việc giảm tải cho cơ sở dữ liệu và tăng tốc độ phản hồi

* Triển khai CloudFront để tối ưu phân phối nội dung
  * Tạo CloudFront distribution cho trang web tĩnh được lưu trữ trên S3
  * Hiểu cách CloudFront sử dụng hệ thống edge locations để tăng tốc độ tải nội dung toàn cầu, cải thiện trải nghiệm người dùng

* Hình thành tư duy tổng thể về cách các dịch vụ AWS liên kết với nhau
  * Route 53 dùng để ánh xạ tên miền, CloudFront tăng tốc phân phối nội dung, DynamoDB và Redis hỗ trợ xử lý dữ liệu hiệu quả
  * Hiểu cách các dịch vụ này kết hợp nhằm mang lại khả năng mở rộng, tin cậy, và hiệu năng cao cho kiến trúc ứng dụng hiện đại


