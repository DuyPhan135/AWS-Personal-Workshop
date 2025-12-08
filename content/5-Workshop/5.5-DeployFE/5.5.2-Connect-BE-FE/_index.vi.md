---
title : "Cấu hình để kết nối giữa Frontend và Backend"
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---
#### Cấu hình ALB để chấp nhận traffic từ HTTPS.
Truy cập EC2, chọn Load Balancer. Chọn ALB của Backend.
![alb](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb.png)

Chọn Add listener để cấu hình ALB lắng nghe HTTPS 443.
![alb2](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb2.png)

Chọn HTTS và nhập Port là 443
![alb3](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb3.png)

Tích chọn Forward to target groups , chọn group của Backend của bạn.
![alb4](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb4.png)

Ở phần Certificate Source, chọn From ACM và chọn Certificate bạn đã tạo trước đó. Và nhấn Add listener.
![alb5](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb5.png)

Tiếp theo là cấu hình Security Group của ALB để cho phép traffic từ HTTPS 443.
Truy cập vào Securiry Group của EC2. Chọn Security Group của ALB.
![alb6](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb6.png)

Chọn Edit inbound rules.
![alb7](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb7.png)

Thêm HTTPS. Rồi nhấn Save rules.
![alb8](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/alb8.png)

Chúng ta đã thiết lập HTTPS cho domain và cấu hình ALB chấp nhận traffic HTTPS (bao gồm từ Amplify) để chuyển tiếp đến ECS.


#### Cấu hình Rewrites and redirect cho Amplify 
Bước tiếp theo, chúng ta quay lại Amplify để cấu hình Frontend sao cho khi gửi request API, ứng dụng Frontend sẽ gọi đến Backend qua endpoint HTTPS của ALB: https://api.webchat.mom.

Chúng ta sẽ vào lại Amplify , chọn Hosting -> Rewrites and redirects.
![amplify-rewrite](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite.png)

Đây là các Rewrite and directs ban đầu.
![amplify-rewrite1](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite1.png)

Trong phần Rewrites and redirects hiện tại thì nếu bạn gọi bất kì đường dẫn nào thì nó sẽ trả về 1 trang index.html.
![amplify-rewrite2](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite2.png)

Chúng ta có thể thấy rằng khi Frontend gọi đến đường dẫn /api/auth/login, Amplify áp dụng rule trong mục Rewrites and Redirects và rewrite request đó về file index.html. Vì vậy response trả về chính là trang index.html, như minh họa bên dưới.
![amplify-rewrite3](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite3.png)

Chúng ta sẽ quay lại mục Rewrites and Redirects của Amplify để cấu hình sao cho các request API được chuyển tiếp đến domain của ALB, thay vì bị rewrite về trang index.html như hiện tại.

Trong Rewrites and redirects , ta chọn Manage redirects.
![amplify-rewrite4](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite4.png)

Chúng ta sẽ xóa hết các dòng cũ và thêm vào dòng mới như hình dưới .Rồi nhấn Save.
![amplify-rewrite5](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite5.png)

Theo hình dưới, ý nghĩa của từng rule như sau:

Dòng 1: Khi Frontend gửi request tới đường dẫn /api/*, Amplify sẽ rewrite request đó sang https://api.webchat.mom/api/\*.

Dòng 2: Khi Frontend truy cập bất kỳ đường dẫn nào khác (/*), Amplify sẽ trả về file gốc / (tức là index.html) của ứng dụng.”
![amplify-rewrite6](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite6.png)

Truy cập vào lại trang web và nhập các trường và tiến hành đăng nhập.
![amplify-rewrite7](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite7.png)

Ở hình dưới, trong tab Network, chúng ta có thể thấy response của request login đã trả về một JSON từ Backend thay vì một trang HTML như trước.
![amplify-rewrite8](/images/5-Workshop/5.5-DeployFE/5.5.3-Connect-BE-FE/amplify-rewrite8.png)

Vậy là chúng ta đã thực hiện xong các bước cần thiết để Frontend và Backend có thể kết nối với nhau. Tiếp theo chúng ta sẽ tinh chỉnh tên miền để người dùng có thể dễ tìm kiếm.