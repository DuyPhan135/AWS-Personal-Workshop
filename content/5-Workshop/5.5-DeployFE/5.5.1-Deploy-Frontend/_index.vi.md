---
title : "Triển khai Frontend với Amplify"
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---
#### Chuẩn bị Github Repository 
Chúng ta sẽ chuẩn bị 1 Repository Github , Project này có dạng Monoporo , trong project có chứa thư mục frontend và thư mục backend . Ở đây chúng ta sẽ triển khai ở thư mục frontend lên Amplify.

![github-repo](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/github-repo.png)

#### Bắt đầu triển khai với Amazon Amplify

Trong AWS Console , truy cập AWS Amplify rồi chọn Deploy an app.
![amplify-deploy](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy.png)

Tiếp tục chọn nền tảng quản lý mã nguồn của bạn, ở đây mình sử dụng Github nên sẽ chọn Github, sau đó nhấn Next.
![amplify-deploy1](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy1.png)

Amplify sẽ hiện 1 cửa sổ mới , với nội dung yêu cầu cấp quyền GitHub để xác thực cho AWS Amplify.
![amplify-deploy2](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy2.png)

Tiếp tục chọn organizations chứa repository mà bạn cần deploy.
![amplify-deploy3](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy3.png)

Ở đây mình chỉ cài đặt cho 1 repository nhất định nên sẽ chọn “Only select repositories” và tiến hành chọn repo mà mình cần deploy , sau đó nhấn “Install & Authorize”.
![amplify-deploy4](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy4.png)

Tại màn hình dưới , bạn sẽ chọn repository của mình (1) và chọn nhánh (2) . Tùy vào cấu trúc thư mục của bạn , nếu thư mục gốc repository của bạn là thư mục chứa mã nguồn chính cần deploy thì bạn sẽ không cần phải tích vào “My app is monorepo” , nhưng đối với repository của mình ,thì mã nguồn chính cần deploy nằm trong thư mục frontend trong thư mục gốc , nên mình sẽ tích chọn (3) và nhập thư mục frontend (4) để giúp Amplify xác định thư mục cần tập trung. Sau đó nhấn Next.
![amplify-deploy5](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy5.png)

Hầu hết trong phần này Amplify sẽ tự động chọn cho bạn. Bạn tiến hành nhập tên App và có thể kiểm tra trước khi Next.
![amplify-deploy6](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy6.png)

Bạn có thể bấm vào Advanced Settings để xem và tinh chỉnh môi trường build Frontend của bạn cho phù hợp với dự án . Bạn chỉ cần kiểm tra lại và nhấn Next.
![amplify-deploy7](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy7.png)

Kiểm tra lại các thông tin quan trọng và nhấn Save and deploy.
![amplify-deploy8](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy8.png)

Đợi vài phút để Amplify tiến hành deploy.
![amplify-deploy9](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy9.png)

Sau khi Deploy thành công ,Amplify sẽ cung cấp cho chúng ta 1 Domain ngẫu nhiên , tùy theo nhu cầu chúng ta sẽ sử dụng Domain này hoặc có thể thêm 1 Custom Domain theo ý muốn của mình.
![amplify-deploy10](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy10.png)

Chúng ta sẽ tiến hành copy Domain đó và paste ra trình duyệt để xem kết quả.
![amplify-deploy11](/images/5-Workshop/5.5-DeployFE/5.5.1-Deploy-FE-Amplify/amplify-deploy11.png)

Như vậy là bạn đã triển khai Frontend lên Amazon Amplify thành công và có thể truy cập bằng domain do Amplify cung cấp. Từ giờ, mỗi lần bạn cập nhật code trên GitHub (nhánh frontend), Amplify sẽ tự động build và deploy lại sau vài phút.

Vậy bước tiếp theo là: kết nối Frontend với Backend đang chạy trên ECS qua ALB. Hiện tại ALB trả về endpoint HTTP, trong khi Amplify phục vụ Frontend qua HTTPS. Điều này gây ra lỗi mixed content và trình duyệt sẽ chặn request.

Ở phần tiếp theo, chúng ta sẽ thiết lập HTTPS cho ALB để Frontend và Backend giao tiếp an toàn và đúng chuẩn.