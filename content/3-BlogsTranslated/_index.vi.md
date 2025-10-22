---
title: "Các bài blogs đã dịch"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

### [Blog 1 - Bộ nhớ đệm ngoại tuyến với AWS Amplify, TanStack, AppSync và MongoDB Atlas](3.1-Blog1/)

Bài viết trình bày cách xây dựng ứng dụng offline-first với giao diện người dùng lạc quan (optimistic UI) sử dụng AWS Amplify, AWS AppSync, TanStack Query và MongoDB Atlas. Ứng dụng mẫu to-do minh họa cách hiển thị tức thì kết quả CRUD trên giao diện trước khi hoàn tất yêu cầu tới server, cải thiện trải nghiệm người dùng. TanStack Query quản lý bộ nhớ đệm cục bộ, đảm bảo dữ liệu khả dụng khi offline, trong khi AWS Amplify và AppSync cung cấp framework full-stack và API GraphQL. MongoDB Atlas đảm nhận lưu trữ dữ liệu, kết hợp với AWS Lambda và Cognito cho tính năng serverless và quản lý người dùng. Ứng dụng triển khai dễ dàng qua Amplify Gen 2, hỗ trợ đồng bộ hóa dữ liệu và xử lý xung đột đơn giản theo cơ chế "first-come, first-served", phù hợp với các ứng dụng có ít xung đột cập nhật.

### [Blog 2 - Giới thiệu chuỗi họp cộng đồng AWS CDK](3.2-Blog2/)

AWS công bố chuỗi họp cộng đồng mới cho AWS Cloud Development Kit (CDK), nhằm tạo cơ hội cho các nhà phát triển, từ người mới đến chuyên gia, học hỏi, đặt câu hỏi và chia sẻ phản hồi trực tiếp với đội ngũ CDK. Các buổi họp diễn ra hai lần mỗi quý, được tổ chức trực tuyến để đảm bảo tính bao quát và dễ tiếp cận, với nội dung bao gồm cập nhật lộ trình, demo tính năng, đánh giá đề xuất, và Q&A mở. Buổi họp đầu tiên dự kiến vào ngày 24/6/2025, với hai khung giờ (8h-9h và 17h-18h PDT) để hỗ trợ cộng đồng toàn cầu. Thông tin chi tiết, tài liệu và bản ghi sẽ được đăng trên GitHub và YouTube. Cộng đồng có thể tham gia qua Slack, đề xuất chủ đề trên GitHub, và đóng góp ý kiến qua khảo sát để định hình tương lai của CDK.

### [Blog 3 - Bảo mật API ứng dụng Express trong 5 phút với Cedar](3.3-Blog3/)

Bài viết giới thiệu gói `authorization-for-expressjs` của dự án mã nguồn mở Cedar, cho phép tích hợp kiểm tra phân quyền dựa trên chính sách vào ứng dụng Express trên Node.js chỉ trong vài phút, giảm 90% mã so với cách tích hợp thủ công. Sử dụng mẫu ứng dụng PetStore, Cedar tách biệt logic phân quyền khỏi mã ứng dụng, cho phép định nghĩa chính sách chi tiết thông qua ngôn ngữ Cedar, ví dụ: chỉ nhân viên được phép thực hiện các thao tác ghi (POST /pets, POST /pets/{petId}/sale), trong khi khách hàng chỉ được đọc (GET /pets, GET /pets/{petId}). Gói này tự động tạo lược đồ Cedar từ đặc tả OpenAPI, sinh chính sách mẫu và tích hợp middleware để kiểm tra phân quyền mà không cần gọi dịch vụ từ xa. Ứng dụng được bảo mật thông qua JWT và OIDC, với khả năng kiểm tra chính sách dễ dàng bằng lệnh `curl`, nâng cao hiệu suất phát triển và đơn giản hóa kiểm tra quyền truy cập.

### [Blog 4 - Kiến Trúc Cơ Sở Amazon Bedrock Trong Một Vùng Hạ Tầng AWS](3.4-Blog4/)

Bài viết mô tả một kiến trúc tham chiếu để quản lý và bảo mật quyền truy cập vào các khả năng của Amazon Bedrock trong môi trường AWS đa tài khoản, hay còn gọi là AWS landing zone. Kiến trúc tập trung này bao gồm ba loại tài khoản chính: tài khoản mạng dịch vụ, tài khoản Generative AI và các tài khoản khối lượng công việc. Giải pháp tận dụng Amazon VPC Lattice để đơn giản hóa việc kết nối, bảo mật và giám sát giao tiếp giữa các dịch vụ. Cách tiếp cận này cung cấp một chiến lược bảo vệ đa lớp, thực thi kiểm soát truy cập chi tiết thông qua các chính sách bảo mật cấp mạng và chính sách xác thực của VPC Lattice, đồng thời nhấn mạnh tầm quan trọng của việc giám sát sử dụng và tuân thủ bằng các công cụ như Amazon CloudWatch và AWS CloudTrail.

### [Blog 5 - Hiện đại hóa Quy trình Mua sắm SAP với Amazon Appflow, SAP BTP Integration Suite và Amazon Bedrock](3.5-Blog5/)

Bài viết trình bày một giải pháp hiện đại hóa quy trình mua sắm của SAP bằng cách tích hợp các dịch vụ AWS như Amazon AppFlow và Amazon Bedrock với SAP BTP Integration Suite. Kiến trúc này tự động hóa và cải tiến quy trình làm việc mua sắm, đặc biệt tập trung vào một trường hợp sử dụng cho Trợ lý Tìm nguồn cung ứng Bền vững. Hệ thống sử dụng giao diện chatbot và AI tạo sinh để phân tích báo giá dựa trên cả chỉ số về giá cả và tính bền vững. Bằng cách tận dụng Amazon Bedrock để xử lý ngôn ngữ tự nhiên và tích hợp với các hệ thống SAP, giải pháp cung cấp một phương pháp tiếp cận linh hoạt, không máy chủ để chuyển đổi các quy trình mua sắm, giúp chúng hiệu quả hơn và phù hợp với các mục tiêu bền vững.

### [Blog 6 - Tối Ưu Hóa Thời Gian Ngừng Hoạt Động Chuyển Đổi Unicode Cho Hệ Thống SAP Trên Oracle Sang AWS](3.6-Blog6/)

Bài viết thảo luận về một phương pháp nhằm giảm thiểu thời gian ngừng hoạt động trong quá trình chuyển đổi Unicode của các hệ thống SAP trên cơ sở dữ liệu Oracle khi di chuyển sang AWS. Bài viết nêu bật một nghiên cứu điển hình của Bell Canada, công ty đã di chuyển thành công hệ thống SAP ERP không phải Unicode 11 TB sang AWS trong vòng chưa đầy 5 giờ, giảm 75% thời gian ngừng hoạt động kỹ thuật so với quy trình chuyển đổi và di chuyển một bước truyền thống. Cách tiếp cận này bao gồm việc thiết lập một hệ thống SAP không phải Unicode tạm thời trên Amazon EC2 và sao chép dữ liệu từ hệ thống tại chỗ bằng Oracle Data Guard. Trong thời gian ngừng hoạt động theo kế hoạch, phiên bản tạm thời này sẽ trở thành nguồn cho việc chuyển đổi Unicode, với các quy trình xuất và nhập chạy song song trên các phiên bản EC2 khác nhau. Phương pháp này, kết hợp với các phương pháp hay nhất như kiểm tra trước và đảm bảo độ tin cậy của mạng, tận dụng cơ sở hạ tầng có thể mở rộng của AWS để giảm thiểu sự gián đoạn kinh doanh và tối ưu hóa hiệu quả hoạt động.

### [Blog 7 - ...](3.7-Blog7/)

### [Blog 8 - ...](3.8-Blog8/)

### [Blog 9 - ...](3.9-Blog9/)

### [Blog 10 - ...](3.10-Blog10/)

### [Blog 11 - ...](3.11-Blog11/)

### [Blog 12 - ...](3.12-Blog12/)
