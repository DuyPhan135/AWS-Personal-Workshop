---
title: "Blog 4"
weight: 1
chapter: false
pre: " <b> 3.4. </b> "
---

# Tăng tốc nghiên cứu HPC và AI trong các trường đại học với Amazon SageMaker HyperPod

Các trường đại học nghiên cứu tham gia vào lĩnh vực AI quy mô lớn và điện toán hiệu năng cao (HPC) thường phải đối mặt với những thách thức hạ tầng đáng kể làm cản trở sự đổi mới và trì hoãn kết quả nghiên cứu. Các cụm HPC truyền thống tại chỗ đi kèm với chu kỳ mua sắm GPU dài, giới hạn mở rộng cứng nhắc và yêu cầu bảo trì phức tạp. Những trở ngại này hạn chế khả năng của các nhà nghiên cứu trong việc lặp lại nhanh chóng các khối lượng công việc AI như xử lý ngôn ngữ tự nhiên (NLP), thị giác máy tính và đào tạo mô hình nền tảng (FM). [Amazon SageMaker HyperPod](https://aws.amazon.com/sagemaker-ai/hyperpod/) giảm bớt gánh nặng không phân biệt được liên quan đến việc xây dựng các mô hình AI. Nó giúp nhanh chóng mở rộng quy mô các tác vụ phát triển mô hình như đào tạo, tinh chỉnh hoặc suy luận trên một cụm hàng trăm hoặc hàng nghìn bộ tăng tốc AI (GPU NVIDIA H100, A100 và others) được tích hợp với các công cụ HPC được cấu hình sẵn và tự động mở rộng quy mô.

### Tổng quan về Giải pháp

Amazon SageMaker HyperPod được thiết kế để hỗ trợ các hoạt động máy học quy mô lớn cho các nhà nghiên cứu và nhà khoa học ML. Dịch vụ được quản lý hoàn toàn bởi AWS, loại bỏ chi phí vận hành trong khi vẫn duy trì bảo mật và hiệu suất cấp doanh nghiệp.

Sơ đồ kiến trúc sau minh họa cách truy cập SageMaker HyperPod để gửi job. Người dùng cuối có thể sử dụng [AWS Site-to-Site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html), [AWS Client VPN](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/what-is.html) hoặc [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) để truy cập an toàn vào cụm SageMaker HyperPod. Các kết nối này kết thúc tại Network Load Balancer, nơi phân phối hiệu quả lưu lượng SSH đến các node đăng nhập, là điểm vào chính để gửi job và tương tác với cụm. Cốt lõi của kiến trúc là SageMaker HyperPod compute, một node điều khiển (controller node) điều phối các hoạt động của cụm và nhiều node tính toán được sắp xếp theo cấu hình lưới. Thiết lập này hỗ trợ các khối lượng công việc đào tạo phân tán hiệu quả với các kết nối tốc độ cao giữa các node, tất cả đều nằm trong một subnet riêng để tăng cường bảo mật.

Cơ sở hạ tầng lưu trữ được xây dựng xung quanh hai thành phần chính: [Amazon FSx for Lustre](https://aws.amazon.com/fsx/lustre/) cung cấp khả năng hệ thống file hiệu suất cao và [Amazon S3](https://aws.amazon.com/s3/) cho lưu trữ chuyên dụng cho tập dữ liệu và checkpoint. Cách tiếp cận lưu trữ kép này cung cấp cả khả năng truy cập dữ liệu nhanh cho khối lượng công việc đào tạo và tính liên tục an toàn của các artifacts đào tạo có giá trị.

![Diagram](/images/3-BlogsTranslated/3.4-Blog4/SMHP-Solution-Architecture.png)

Việc triển khai bao gồm một số giai đoạn. Trong các bước sau, chúng tôi trình bày cách triển khai và cấu hình giải pháp.

### Điều kiện tiên quyết

Trước khi triển khai Amazon SageMaker HyperPod, hãy đảm bảo các điều kiện tiên quyết sau đã được thiết lập:

- Cấu hình AWS:
  - [AWS Command Line Interface](http://aws.amazon.com/cli) (AWS CLI) được cấu hình với các quyền phù hợp
  - Các file cấu hình cụm đã được chuẩn bị: `cluster-config.json` và `provisioning-parameters.json`
- Thiết lập mạng:
  - Một virtual private cloud (VPC) được cấu hình cho các tài nguyên cụm.
  - Các security group với giao tiếp [Elastic Fabric Adapter](https://aws.amazon.com/hpc/efa/) (EFA) được kích hoạt.
  - Một hệ thống file [Amazon FSx for Lustre](https://aws.amazon.com/fsx/lustre/) để lưu trữ chia sẻ hiệu suất cao
- Một vai trò [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) với các quyền cho những mục sau:
  - Quản lý instance [Amazon Elastic Compute Cloud](http://aws.amazon.com/ec2) (Amazon EC2) và cụm [Amazon SageMaker](https://aws.amazon.com/sagemaker/)
  - Truy cập FSx for Lustre và [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3)
  - Tích hợp [Amazon CloudWatch Logs](http://aws.amazon.com/cloudwatch) và [AWS Systems Manager](https://aws.amazon.com/systems-manager/)
  - Cấu hình mạng EFA

### Khởi chạy stack CloudFormation

Chúng tôi đã khởi chạy một stack [AWS CloudFormation](http://aws.amazon.com/cloudformation) để cung cấp các thành phần cơ sở hạ tầng cần thiết, bao gồm VPC và subnet, hệ thống file FSx for Lustre, bucket S3 cho các script vòng đời và dữ liệu đào tạo, và các vai trò IAM với các quyền được phạm vi cho hoạt động cụm. Tham khảo [Amazon SageMaker HyperPod workshop](https://catalog.workshops.aws/sagemaker-hyperpod/en-US) để biết các mẫu CloudFormation và script tự động hóa.

### Tùy chỉnh cấu hình cụm SLURM

Để căn chỉnh tài nguyên tính toán với nhu cầu nghiên cứu của khoa, chúng tôi đã tạo các partition SLURM để phản ánh cấu trúc tổ chức, ví dụ: các nhóm NLP, thị giác máy tính và học sâu. Chúng tôi đã sử dụng [cấu hình partition SLURM](https://github.com/Microway/MCMS-OpenHPC-Recipe/blob/master/dependencies/etc/slurm/slurm.conf) để định nghĩa slurm.conf với các partition tùy chỉnh. Kế toán SLURM (SLURM accounting) được kích hoạt bằng cách cấu hình `slurmdbd` và liên kết việc sử dụng với các tài khoản khoa và người giám sát.

Để hỗ trợ chia sẻ GPU theo phân số (fractional) và sử dụng hiệu quả, chúng tôi đã kích hoạt cấu hình Generic Resource (GRES). Với GPU stripping, nhiều người dùng có thể truy cập GPU trên cùng một node mà không có sự tranh chấp. Việc thiết lập GRES tuân theo hướng dẫn từ [Amazon SageMaker HyperPod workshop.](https://catalog.workshops.aws/sagemaker-hyperpod/en-US/08-tips-and-tricks/08-gres)

### Cung cấp và xác thực cụm

Chúng tôi đã xác thực các file `cluster-config.json` và `provisioning-parameters.json` bằng cách sử dụng AWS CLI và một script xác thực SageMaker HyperPod:

```bash
$curl -O https://raw.githubusercontent.com/aws-samples/awsome-distributed-training/main/1.architectures/5.sagemaker-hyperpod/validate-config.py

$pip3 install boto3

$python3 validate-config.py --cluster-config cluster-config.json --provisioning-parameters provisioning-parameters.json
```

Sau đó chúng tôi tạo cụm:

```bash
$aws sagemaker create-cluster \
  --cli-input-json file://cluster-config.json \
  --region us-west-2
```

### Triển khai theo dõi chi phí và thực thi ngân sách

Để giám sát việc sử dụng và kiểm soát chi phí, mỗi tài nguyên SageMaker HyperPod (ví dụ: Amazon EC2, FSx for Lustre và others) được gắn thẻ với một thẻ `ClusterName` duy nhất. Các báo cáo [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/) và [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) được cấu hình để theo dõi chi tiêu hàng tháng cho mỗi cụm. Ngoài ra, các cảnh báo được thiết lập để thông báo cho các nhà nghiên cứu nếu họ tiếp cận hạn ngạch hoặc ngưỡng ngân sách của mình.

Sự tích hợp này giúp tạo điều kiện sử dụng hiệu quả và chi tiêu nghiên cứu có thể dự đoán được.

### Kích hoạt cân bằng tải cho các node đăng nhập

Khi số lượng người dùng đồng thời tăng lên, trường đại học đã áp dụng kiến trúc đa node đăng nhập. Hai node đăng nhập được triển khai trong các nhóm EC2 Auto Scaling. Một [Network Load Balancer](https://aws.amazon.com/elasticloadbalancing/network-load-balancer/) được cấu hình với các target group để định tuyến lưu lượng SSH và Systems Manager. Cuối cùng, các hàm [AWS Lambda](http://aws.amazon.com/lambda) thực thi giới hạn phiên cho mỗi người dùng bằng cách sử dụng thẻ `Run-As` với [Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html), một khả năng của Systems Manager.

Để biết chi tiết về triển khai đầy đủ, hãy xem [Implementing login node load balancing in SageMaker HyperPod for enhanced multi-user experience.](https://aws.amazon.com/blogs/machine-learning/implementing-login-node-load-balancing-in-sagemaker-hyperpod-for-enhanced-multi-user-experience/)

### Cấu hình quyền truy cập liên kết và ánh xạ người dùng

Để tạo điều kiện truy cập an toàn và liền mạch cho các nhà nghiên cứu, tổ chức đã tích hợp [AWS IAM Identity Center](https://aws.amazon.com/iam/identity-center/) với Active Directory (AD) tại chỗ của họ bằng cách sử dụng [AWS Directory Service.](https://aws.amazon.com/directoryservice/) Điều này cho phép kiểm soát và quản trị thống nhất danh tính người dùng và đặc quyền truy cập trên các tài khoản SageMaker HyperPod. Việc triển khai bao gồm các thành phần chính sau:

- **Tích hợp người dùng liên kết** – Chúng tôi ánh xạ người dùng AD sang tên người dùng POSIX bằng cách sử dụng thẻ `run-as` của Session Manager, cho phép kiểm soát chi tiết quyền truy cập node tính toán
- **Quản lý phiên an toàn** – Chúng tôi đã cấu hình Systems Manager để đảm bảo người dùng truy cập các node tính toán bằng tài khoản của chính họ, không phải `ssm-user` mặc định
- **Gắn thẻ dựa trên danh tính** – Tên người dùng liên kết được tự động ánh xạ đến các thư mục người dùng, khối lượng công việc và ngân sách thông qua các thẻ tài nguyên

Để được hướng dẫn từng bước đầy đủ, hãy tham khảo [Amazon SageMaker HyperPod workshop.](https://catalog.workshops.aws/sagemaker-hyperpod/en-US/08-tips-and-tricks/01-multi-user)

Cách tiếp cận này đã hợp lý hóa việc cung cấp người dùng và kiểm soát truy cập trong khi vẫn duy trì sự phù hợp mạnh mẽ với các chính sách và yêu cầu tuân thủ của tổ chức.

### Tối ưu hóa sau triển khai

Để giúp ngăn chặn việc tiêu thụ không cần thiết tài nguyên tính toán bởi các phiên không hoạt động, trường đại học đã cấu hình SLURM với [Pluggable Authentication Modules](https://github.com/SchedMD/slurm/tree/master/contribs/pam) (PAM). Thiết lập này thực thi việc đăng xuất tự động cho người dùng sau khi job SLURM của họ hoàn thành hoặc bị hủy, hỗ trợ tính khả dụng kịp thời của các node tính toán cho các job đang xếp hàng.

Cấu hình này đã cải thiện thông lượng lập lịch job bằng cách giải phóng ngay lập tức các node nhàn rỗi và giảm chi phí quản lý trong việc quản lý các phiên không hoạt động.

Ngoài ra, các chính sách [QoS](https://github.com/SchedMD/slurm/blob/master/doc/html/qos.shtml) được cấu hình để kiểm soát mức tiêu thụ tài nguyên, giới hạn thời lượng job và thực thi quyền truy cập GPU công bằng trên tất cả người dùng và các khoa. Ví dụ:

- **MaxTRESPerUser** – Đảm bảo việc sử dụng GPU hoặc CPU trên mỗi người dùng nằm trong giới hạn đã định
- **MaxWallDurationPerJob** – Giúp ngăn chặn các job quá dài độc chiếm các node
- **Trọng số ưu tiên** – Căn chỉnh lập lịch ưu tiên dựa trên nhóm nghiên cứu hoặc dự án

Những cải tiến này tạo điều kiện cho một môi trường HPC được tối ưu hóa, cân bằng, phù hợp với mô hình cơ sở hạ tầng chia sẻ của các tổ chức nghiên cứu học thuật.

### Dọn dẹp

Để xóa các tài nguyên và tránh phát sinh chi phí liên tục, hãy hoàn thành các bước sau:

1. Xóa cụm SageMaker HyperPod:

```bash
$aws sagemaker delete-cluster --cluster-name <tên-cụm>
```

2. Xóa stack CloudFormation được sử dụng cho cơ sở hạ tầng SageMaker HyperPod:

```bash
$aws cloudformation delete-stack --stack-name <tên-stack> --region <vùng>
```

Thao tác này sẽ tự động xóa các tài nguyên liên quan, chẳng hạn như VPC và subnets, hệ thống file FSx for Lustre, bucket S3 và các vai trò IAM. Nếu bạn đã tạo các tài nguyên này bên ngoài CloudFormation, bạn phải xóa chúng theo cách thủ công.

### Kết luận

SageMaker HyperPod cung cấp cho các trường đại học nghiên cứu một giải pháp HPC mạnh mẽ, được quản lý hoàn toàn, được thiết kế cho các nhu cầu riêng biệt của khối lượng công việc AI. Bằng cách tự động hóa việc cung cấp cơ sở hạ tầng, mở rộng quy mô và tối ưu hóa tài nguyên, các tổ chức có thể tăng tốc đổi mới trong khi vẫn duy trì kiểm soát ngân sách và hiệu quả hoạt động. Thông qua các cấu hình SLURM tùy chỉnh, chia sẻ GPU bằng GRES, quyền truy cập liên kết và cân bằng node đăng nhập mạnh mẽ, giải pháp này làm nổi bật tiềm năng của SageMaker HyperPod trong việc chuyển đổi điện toán nghiên cứu, giúp các nhà nghiên cứu tập trung vào khoa học, không phải cơ sở hạ tầng.