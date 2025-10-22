---
title: "Blog 5"
weight: 1
chapter: false
pre: " <b> 3.5. </b> "
---

# Hiện đại hóa Quy trình Mua sắm SAP với Amazon Appflow, SAP BTP Integration Suite và Amazon Bedrock


Nhiều khách hàng của chúng tôi đang tìm kiếm hướng dẫn về cách AI tạo sinh có thể hỗ trợ chiến lược hiện đại hóa toàn doanh nghiệp của họ. [Amazon Bedrock](https://aws.amazon.com/bedrock/) là một dịch vụ được quản lý hoàn toàn cung cấp quyền truy cập vào một loạt các mô hình nền tảng (FMs) hiệu suất cao từ các công ty AI hàng đầu như [Anthropic](https://www.anthropic.com/), [AI21 Labs](https://www.ai21.com/), [Cohere](https://cohere.com/), [Stability AI](https://stability.ai/), [Mistral](https://aws.amazon.com/bedrock/mistral/), [Meta Llama](https://aws.amazon.com/bedrock/llama/), [Amazon Nova](https://aws.amazon.com/ai/generative-ai/nova/?gclid=EAIaIQobChMI97Keq4HkigMVxJVQBh1uWQx-EAAYASAAEgJatvD_BwE&trk=42edd3ad-f9d1-4bec-98be-19a453395a44&sc_channel=ps&ef_id=EAIaIQobChMI97Keq4HkigMVxJVQBh1uWQx-EAAYASAAEgJatvD_BwE:G:s&s_kwcid=AL!4422!3!691967596755!e!!g!!amazon%20nova!21048269259!166106444064) và [Amazon Titan](https://aws.amazon.com/bedrock/titan/).

Amazon Bedrock cung cấp một bộ tính năng toàn diện để xây dựng các ứng dụng AI tạo sinh, đơn giản hóa phát triển đồng thời duy trì quyền riêng tư và bảo mật. Các tính năng chính bao gồm tùy chỉnh mô hình với dữ liệu của riêng bạn, tinh chỉnh cho các nhiệm vụ cụ thể và [Retrieval Augmented Generation (RAG)](https://aws.amazon.com/what-is/retrieval-augmented-generation/) để nâng cao độ chính xác của phản hồi bằng cách sử dụng cơ sở kiến thức của công ty bạn. Bedrock cũng hỗ trợ xây dựng các tác nhân thông minh có thể tự động hóa các nhiệm vụ bằng cách tương tác với các hệ thống doanh nghiệp của bạn.

Các doanh nghiệp có kỳ vọng cao về bảo mật và độ tin cậy cơ sở hạ tầng, đặc biệt đối với các khối lượng công việc quan trọng như những khối lượng chạy trên SAP. Amazon Bedrock đáp ứng những kỳ vọng này bằng cách tích hợp các biện pháp bảo mật mạnh mẽ, bao gồm mã hóa dữ liệu và tuân thủ các tiêu chuẩn như [SOC](https://aws.amazon.com/compliance/soc-faqs/) và [ISO](https://aws.amazon.com/compliance/iso-27001-faqs/). Điều này tạo điều kiện cho một môi trường bảo mật, làm cho Bedrock trở thành lựa chọn lý tưởng để phát triển các ứng dụng AI sáng tạo đáp ứng các yêu cầu nghiêm ngặt của các giải pháp cấp doanh nghiệp.

**Nâng cao Mua sắm: Sức mạnh của AI Tạo sinh trong Nguồn cung Ứng Bền vững**

Các thực hành mua sắm ngày nay đối mặt với những thách thức đáng kể, đặc biệt là trong quy trình tốn thời gian của việc xem xét nhiều báo giá nhà cung cấp và nhiệm vụ phức tạp của việc thu thập dữ liệu bền vững từ các nguồn khác nhau. AI tạo sinh đang nổi lên như một giải pháp biến đổi, với các ví dụ như Trợ lý Nguồn cung Ứng Bền vững (được cung cấp bởi Amazon Bedrock) chứng minh cách AI có thể tận dụng cả dữ liệu SAP & Non-SAP để biến đổi các quy trình mua sắm và nâng cao các sáng kiến bền vững xuyên suốt chuỗi cung ứng. Sử dụng Amazon Bedrock, khách hàng SAP có thể cung cấp các lệnh nhắc ngôn ngữ tự nhiên để phân tích thông tin báo giá từ các hệ thống SAP. Sau đó, nó kết hợp dữ liệu này với các chỉ số bền vững non-SAP để khuyến nghị báo giá tối ưu dựa trên các tiêu chí bền vững cụ thể theo doanh nghiệp. Hệ thống cũng cho phép người dùng tạo đơn đặt hàng mua trong SAP bằng các lệnh ngôn ngữ tự nhiên, đơn giản hóa toàn bộ quy trình mua sắm.

Ví dụ này giải quyết các thách thức chính mà các chuyên gia nguồn cung ứng bền vững phải đối mặt bao gồm:

* Việc xem xét báo giá tốn thời gian, nhu cầu truy cập nhiều nguồn dữ liệu bên ngoài.
* Khó khăn trong việc thu thập thông tin bền vững toàn diện.
* Khó khăn trong việc phù hợp báo giá với các mục tiêu tổ chức cụ thể (trong trường hợp này, là bền vững).

## Tổng quan Giải pháp

Khám phá Tiềm năng của AI trong Mua sắm: **Trường hợp Sử dụng Trợ lý Nguồn cung Ứng Bền vững**

Trong bài đăng blog này, chúng tôi sẽ khám phá một ví dụ sáng tạo về cách AI có thể biến đổi các quy trình kinh doanh thông qua một trợ lý được cung cấp bởi AI khái niệm. Điều quan trọng cần lưu ý rằng đây không phải là một sản phẩm hoặc dịch vụ sẵn sàng sử dụng, mà là một minh họa về cách các công nghệ khác nhau có thể được kết hợp để giải quyết các thách thức kinh doanh phổ biến trong mua sắm. Trường hợp sử dụng này đóng vai trò là nguồn cảm hứng cho cách các cách tiếp cận tương tự có thể được áp dụng và tùy chỉnh cho các lĩnh vực chức năng khác nhau trong tổ chức của bạn, dù là sản xuất, tài chính, nguồn nhân lực hoặc các hoạt động kinh doanh khác.

Xuyên suốt bài đăng blog này, chúng tôi sẽ thảo luận về các lựa chọn kiến trúc khác nhau và các dịch vụ AWS có thể được triển khai thông qua sự kết hợp của công nghệ chatbot, xử lý ngôn ngữ tự nhiên, Amazon Bedrock Agents và tích hợp với [SAP BTP Integration Suite](https://www.sap.com/products/technology-platform/integration-suite.html). Trong khi chúng tôi tập trung vào một ví dụ cụ thể, sự linh hoạt này cho phép các tổ chức chọn các thành phần phù hợp nhất với yêu cầu của họ, cơ sở hạ tầng hiện có và sở thích kỹ thuật, làm cho các nguyên tắc và công nghệ cơ bản có thể thích ứng với một loạt các quy trình kinh doanh và trường hợp sử dụng.

**Lợi ích Tiềm năng của Mua sắm Hỗ trợ bởi AI**

Trong trường hợp sử dụng ví dụ của chúng tôi, chúng tôi chứng minh cách AI tạo sinh có thể được sử dụng để:

1. **Đơn giản hóa Đánh giá**: Tự động hóa quy trình đánh giá báo giá, có khả năng giảm thời gian dành cho các đánh giá.
2. **Tích hợp Dữ liệu**: Khai thác cả nguồn dữ liệu nội bộ và bên ngoài, cung cấp thông tin toàn diện hơn cho việc ra quyết định.
3. **Nâng cao Tính Khách quan**: Sử dụng các cách tiếp cận được cung cấp bởi AI Tạo sinh để hỗ trợ các quy trình lựa chọn không thiên vị tuân thủ các nguyên tắc được định nghĩa trước.
4. **Quy trình Đơn giản hóa**: AI Tạo sinh có thể giúp tự động hóa các bước tiếp theo, chẳng hạn như tạo đơn đặt hàng mua, chuyển tiếp liền mạch từ Yêu cầu Báo giá (RFQ) sang tạo đơn đặt hàng mua.
5. **Giảm lỗi con người**: AI Tạo sinh có thể nâng cao độ chính xác và tính nhất quán trong các quy trình kinh doanh bằng cách tự động xác thực đầu vào dữ liệu, xác định các mẫu lỗi và cung cấp hướng dẫn thời gian thực cho người dùng trước khi các sai lầm được thực hiện.

Bây giờ hãy đi sâu vào kiến trúc kỹ thuật đằng sau trường hợp sử dụng mua sắm được cung cấp bởi AI khái niệm của chúng tôi. Ví dụ này chứng minh cách các dịch vụ AWS và công nghệ SAP khác nhau có thể được kết hợp để tạo ra một công cụ mạnh mẽ cho các quy trình mua sắm. Chúng tôi sẽ sử dụng giao diện kiểu chatbot để truy vấn RFQ theo thời gian thực gần, bổ sung dữ liệu SAP này với dữ liệu bền vững bên thứ ba, và cuối cùng tạo đơn đặt hàng mua bằng trợ lý. Một lần nữa, điều quan trọng cần lưu ý rằng đây không phải là một sản phẩm sẵn sàng sử dụng, mà là một minh họa về những gì có thể khi tận dụng các công nghệ này.

## Kiến trúc Chi tiết:

![high level steps & architecture diagram](/images/3-BlogsTranslated/3.5-Blog5/image-1-1.png)

<div align="center">
  <i>high level steps & architecture diagram</i>
</div>

Giải pháp này đã được xây dựng và kiểm tra trên [SAP S/4HANA 2023](https://help.sap.com/doc/e2048712f0ab45e791e6d15ba5e20c68/2023/en-US/FSD_OP2023_latest.pdf) và có thể được triển khai trên các lựa chọn triển khai khác nhau bao gồm RISE with SAP, SAP on AWS, Cài đặt AWS Bản địa hoặc môi trường tại chỗ. Kiến trúc giải pháp chứng minh một cách tiếp cận linh hoạt để triển khai, cho phép các tổ chức thích ứng các thành phần dựa trên nhu cầu cụ thể và cảnh quan công nghệ hiện có của họ. Trong khi ví dụ của chúng tôi thể hiện một sự kết hợp cụ thể của các dịch vụ AWS và SAP, điều quan trọng cần hiểu rằng có ba lớp chính nơi các công nghệ thay thế có thể được sử dụng mà không làm giảm chức năng tổng thể của giải pháp.

**Các Thành phần Cốt lõi và Các Lựa chọn Thay thế của Chúng:**

1. **Lớp Tích hợp Dữ liệu**
   - Ví dụ của Chúng tôi: Amazon AppFlow với SAP OData Connector
   - Lựa chọn Thay thế [AWS Glue connection for SAP](https://aws.amazon.com/blogs/big-data/scaling-rise-with-sap-data-and-aws-glue/) (sử dụng ODATA)
   - Mục đích: Xử lý trích xuất dữ liệu thời gian thực gần từ các hệ thống SAP
2. **Lớp Xử lý & Trí tuệ**
   - Ví dụ của Chúng tôi: [Amazon Bedrock với chức năng Agent](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-action-create.html)
   - Lựa chọn Thay thế: [SAP Generative AI Hub](https://help.sap.com/docs/sap-ai-core/sap-ai-core-service-guide/generative-ai-hub-in-sap-ai-core) (với lựa chọn mô hình)
   - Mục đích: Quản lý xử lý AI và điều phối
3. **Lớp Giao diện Người dùng**
   - Ví dụ của Chúng tôi: [Ứng dụng Streamlit](https://aws.amazon.com/blogs/opensource/using-streamlit-to-build-an-interactive-dashboard-for-data-analysis-on-aws/)
   - Lựa chọn Thay thế: [SAP AI Core và Build Apps](https://community.sap.com/t5/devtoberfest/chatbot-development-made-easy-with-sap-ai-core-and-build-apps/ev-p/13795574)
   - Mục đích: Cung cấp giao diện người dùng cho tương tác

**Luồng Kiến trúc (Tham chiếu trong high level steps & architecture diagram)**:

1. Trích xuất Dữ liệu: Amazon AppFlow kết nối với SAP OData Connector cho trích xuất dữ liệu thời gian thực gần
2. Lưu trữ Dữ liệu: Amazon S3 lưu trữ dữ liệu được trích xuất ở định dạng JSON
3. Giao diện Người dùng: Một trợ lý chatbot nhận đầu vào ngôn ngữ tự nhiên từ người dùng
4. Xử lý: Bedrock Agent diễn giải đầu vào người dùng, tận dụng lịch sử trò chuyện và Mô hình Nền tảng cơ bản
5. Điều phối Hành động: Bedrock Agent được cấu hình với Các Nhóm Hành động để quản lý các bước xử lý
6. Truy vấn Dữ liệu: Các hàm Lambda dịch ngôn ngữ tự nhiên thành các truy vấn SQL cho cơ sở dữ liệu Athena
7. Tích hợp SAP: AWS Lambda gọi [BTP API](https://developers.sap.com/tutorials/api-mgmt-isuite-initial-setup..html) để tạo Đơn đặt hàng Mua trong SAP
8. Kết nối An toàn: [SAP Cloud Connector](https://help.sap.com/docs/connectivity/sap-btp-connectivity-cf/cloud-connector?locale=en-US) tạo một đường hầm an toàn giữa các tài khoản AWS và BTP
9. Nâng cao Kiến thức: Cơ sở Kiến thức của Amazon Bedrock cung cấp RAG được quản lý cho ngữ cảnh bổ sung
10. Chuẩn bị Dữ liệu: Dữ liệu bucket S3 được đồng bộ và biến đổi thành các embedding cho sử dụng học máy
11. Tạo Phản hồi: Agent biên soạn một phản hồi cuối cùng, được giao qua ứng dụng Streamlit

**Cơ sở Hạ tầng Hỗ trợ:** 

* Lưu trữ Dữ liệu: Amazon S3 (định dạng JSON)
* Xử lý Truy vấn: AWS Lambda với Amazon Athena
* Thành phần Tích hợp: SAP BTP Integration Suite, SAP Cloud Connector
* Quản lý Kiến thức: Cơ sở Kiến thức Amazon Bedrock cho RAG
* Chuẩn bị Dữ liệu: Bucket S3 với biến đổi embedding

Cách tiếp cận mô-đun này cho phép các tổ chức kết hợp và ghép các thành phần dựa trên ngăn xếp công nghệ hiện có, sở thích và yêu cầu của họ đồng thời duy trì chức năng cốt lõi của giải pháp.

Hãy khám phá các dịch vụ AWS chính cung cấp năng lượng cho giải pháp này và vai trò cụ thể của chúng, trước khi chúng tôi chứng minh Trợ lý đang hoạt động thông qua các ví dụ thực tế.

* Amazon S3
* Amazon AppFlow
* AWS Glue
* Amazon Athena
* AWS Lambda
* Amazon Bedrock

**Amazon S3:** 
S3 hoạt động như một hồ dữ liệu trung tâm, nhận dữ liệu trực tiếp từ S/4HANA qua AppFlow và lưu trữ nó ở các định dạng được tối ưu hóa phân tích bao gồm CSV, JSON hoặc Parquet. Giải pháp kết hợp AppFlow cho việc hấp thụ dữ liệu, S3 cho lưu trữ có thể mở rộng và Athena cho phân tích dựa trên SQL, với AWS Glue lập danh mục dữ liệu để dễ dàng khám phá hơn.

**Amazon AppFlow:** 
[Amazon AppFlow](https://aws.amazon.com/appflow/) trích xuất dữ liệu báo giá và RFQ từ SAP vào Amazon S3 theo thời gian thực gần, sử dụng một bộ kết nối SAP an toàn mà không yêu cầu tích hợp tùy chỉnh. Nó sử dụng Change Data Capture để phát hiện và chuyển chỉ các bản ghi mới hoặc đã sửa đổi, đảm bảo hồ dữ liệu S3 duy trì thông tin hiện tại nhất.

**AWS Glue:** 
AWS Glue crawler tự động khám phá, lập danh mục và tổ chức dữ liệu được lưu trữ trong các bucket S3, làm cho nó dễ dàng tìm kiếm và phân tích. Đây là một dịch vụ ETL được quản lý hoàn toàn đơn giản hóa chuẩn bị dữ liệu và tải cho mục đích phân tích.

**Amazon Athena:** 
Athena là một dịch vụ truy vấn không máy chủ phân tích dữ liệu trực tiếp trong S3 sử dụng SQL tiêu chuẩn, hỗ trợ các định dạng dữ liệu khác nhau và tự động song song hóa các truy vấn cho thực thi phân tán. Nó tích hợp với AWS Glue Data Catalog và hỗ trợ phân tích phức tạp đồng thời duy trì mô hình giá theo truy vấn.

**AWS Lambda:** 
Lambda hoạt động như một trung gian giữa Amazon Bedrock Agents và SAP API, xử lý tích hợp kỹ thuật cho xử lý đơn đặt hàng mua. Nó quản lý xác thực, biến đổi dữ liệu và xử lý lỗi, cho phép xử lý có thể mở rộng, tiết kiệm chi phí của các yêu cầu đơn đặt hàng mua được cung cấp bởi AI.

**Amazon Bedrock:** 
Bedrock, sử dụng Claude 3 Sonnet v1, cung cấp các khả năng xử lý ngôn ngữ tự nhiên tiên tiến để phân tích báo giá dựa trên giá cả và bền vững. Nó cung cấp kiến trúc không máy chủ cho triển khai dễ dàng và bao gồm các tính năng Đánh giá Mô hình để đảm bảo độ chính xác trong các nhiệm vụ như phân tích báo giá.

**Các Xem xét Bảo mật:**

Trong ví dụ blog này, chúng tôi đã giả định các nhóm LDAP qua Đăng nhập Đơn qua để hạn chế truy cập đối với người dùng mua sắm được ủy quyền. Đối với môi trường sản xuất, chúng tôi khuyến nghị triển khai lan truyền danh tính cho bảo mật nâng cao.

**Các Xem xét Chi phí:**

Mặc dù chi phí chính xác sẽ thay đổi dựa trên sử dụng và chi tiết triển khai cụ thể, đây là tổng quan chung:

* SAP Integration Suite: [Thử nghiệm miễn phí 90 ngày](https://help.sap.com/docs/integration-suite/sap-integration-suite/using-free-service-plans), sau đó dựa trên đăng ký. [Liên kết](https://www.sap.com/products/technology-platform/integration-suite/pricing.html#:~:text=SAP%20Integration%20Suite%2C%20additional%20messages,USD%2077.04) đến giá SAP.
* Dịch vụ AWS: Ước tính $900 mỗi tháng cho xử lý 30.000 đơn đặt hàng mua

Điều quan trọng cần lưu ý rằng các chi phí này là minh họa và nên được đánh giá cẩn thận cho trường hợp sử dụng cụ thể của bạn. AWS khuyến nghị theo dõi chặt chẽ chi phí trong bất kỳ khái niệm chứng minh nào và mở rộng tương ứng.

**Tối ưu hóa Chi phí:**

Để tối đa hóa giá trị, hãy xem xét tận dụng [SAP Integration Suite](https://help.sap.com/docs/integration-suite/sap-integration-suite/what-s-new-for-sap-integration-suite) trên nhiều trường hợp sử dụng trong tổ chức của bạn. Cách tiếp cận này có thể giúp đạt được quy mô kinh tế và giúp biện minh cho đầu tư.

## Tổng quan minh họa

Hãy kiểm tra Trợ lý Nguồn cung Ứng Bền vững đang hoạt động. Thông qua một loạt các truy vấn tương tác sử dụng ngôn ngữ tự nhiên, chúng tôi sẽ chứng minh cách trợ lý xử lý và phản hồi các truy vấn liên quan khác nhau. Hệ thống tích hợp dữ liệu SAP, được kéo theo thời gian thực gần qua Amazon AppFlow vào hồ dữ liệu Amazon S3, đồng thời làm phong phú phản hồi với các chỉ số bền vững bên thứ ba từ Cơ sở Kiến thức Bedrock thông qua điều phối Amazon Bedrock Agents.

Bằng cách nhập các lệnh nhắc sau, bắt đầu với “Please List RFQs”, bạn có thể thấy đầu ra.

1. Please list RFQs?

![Image 1](/images/3-BlogsTranslated/3.5-Blog5/image-2-1.png)

2. Were any RFQs created on 2024-09-03?

![Image 2](/images/3-BlogsTranslated/3.5-Blog5/image-3-1.png)

1. How many quotations are received for RFQ 7000000026 and from whom and what is the total cost ?

![Image 3](/images/3-BlogsTranslated/3.5-Blog5/image-4-1.png)

4. Do you have sustainability and other payment term details for these suppliers ?

![Image 4](/images/3-BlogsTranslated/3.5-Blog5/image-5-1.png)

5. Recommend the best supplier for RFQ 7000000026 based on price and sustainability score.

![Image 5](/images/3-BlogsTranslated/3.5-Blog5/image-6-1.png)

6. Create a purchase order for the recommended supplier.

![Image 6](/images/3-BlogsTranslated/3.5-Blog5/image-7-1.png)

7. Create an email to the supplier to negotiate a better price based on other vendors.
![Image 7](/images/3-BlogsTranslated/3.5-Blog5/image-8-1.png)

Trợ lý cũng có thể hỗ trợ các chức năng mua sắm khác nhau, bao gồm các nhiệm vụ giao tiếp tự động. Ví dụ, chúng tôi có thể chứng minh khả năng của nó trong việc soạn thảo thư từ đàm phán nhà cung cấp, tận dụng dữ liệu giá so sánh từ nhiều báo giá nhà cung cấp.

Tất cả điều này có thể thông qua việc cung cấp ngữ cảnh cho Amazon Bedrock Agent. Điều này được thực hiện bằng cách cung cấp hướng dẫn cho Agent. Bạn viết hướng dẫn mô tả những gì agent được thiết kế để làm. Với các lệnh nhắc nâng cao, bạn có thể tùy chỉnh thêm hướng dẫn cho agent ở mọi bước của điều phối và bao gồm các hàm Lambda để phân tích đầu ra của mỗi bước. Ví dụ, trong trường hợp của chúng tôi chúng tôi đã cung cấp những điều sau.

Vai trò: Bạn là Nhà điều hành Tài chính sử dụng hệ thống SAP S/4Hana để truy vấn Yêu cầu Báo giá và phản hồi từ nhà cung cấp đấu thầu.

Mục tiêu:

– Trả về thông tin RFQ và báo giá bằng cách truy vấn athena và trả về dữ liệu dựa trên yêu cầu người dùng được cung cấp.

– Nếu bạn cần tạo đơn đặt hàng mua, thì sử dụng hàm /createPO. Nếu Tạo PO Thất bại hoặc Hoàn thành với lỗi, hãy trả về nêu rõ PO Creation Failed with error.

– Nếu bạn cần trao báo giá, thì sử dụng hàm /awardquotation.

1. Phân tích Truy vấn và Hiểu biết:

– Phân tích yêu cầu của người dùng để hiểu mục tiêu chính.

– Phân tích các yêu cầu thành các truy vấn con mà mỗi truy vấn có thể giải quyết một phần của yêu cầu người dùng, sử dụng schema được cung cấp.

– Luôn sử dụng đầu ra trước đó để xác định đầu vào của các câu hỏi tiếp theo.

2. Tạo Truy vấn SQL:

– Đối với mỗi truy vấn con, sử dụng các bảng và trường liên quan từ schema được cung cấp.

– Xây dựng các truy vấn SQL chính xác và được điều chỉnh để lấy dữ liệu chính xác mà yêu cầu của người dùng yêu cầu.

– Trong trường hợp RFQ hoặc Báo giá trùng lặp. Luôn lấy bản ghi có giá trị tối đa trong cột rfglifecyclestatus và qtnlifecyclestatus

3. Thực thi Truy vấn và Phản hồi:

– Thực thi các truy vấn SQL đã xây dựng đối với cơ sở dữ liệu Amazon Athena.

– Trả về kết quả đảm bảo tính toàn vẹn và độ chính xác của dữ liệu. Không bao gồm tên bảng hoặc mã thực thi truy vấn trong phản hồi.

– Phản hồi với người dùng như thể bạn là một con người và không phải đang thực thi truy vấn từ cơ sở dữ liệu.

– Vui lòng sử dụng tên tự nhiên khi trả về dữ liệu cho người dùng. Ví dụ, thay vì “requestforquotation” hãy sử dụng “request for quotation number”.

– Vui lòng phản hồi ở định dạng tóm tắt.

4. Việc lựa chọn báo giá cho một RFQ cũng nên xem xét chi tiết bền vững và chi tiết điều khoản thanh toán nhận được từ nhà cung cấp cho vật liệu nằm trong cơ sở kiến thức ngoài tổng chi phí của báo giá. Khi được hỏi về báo giá rẻ nhất, luôn xem xét dấu chân carbon bền vững và điều khoản thanh toán của nhà cung cấp ngoài chi phí. Phản hồi với lý luận chi tiết về lý do tại sao một báo giá tốt hơn bằng cách so sánh các báo giá khác, bao gồm lý do bền vững từ cơ sở kiến thức.

5. Hiển thị chi phí ở định dạng đô la, bao gồm cent, với ký hiệu ‘$’ được thêm trước giá trị.


Chúng tôi nhằm cung cấp hướng dẫn Giải pháp AWS với mã mẫu trong tương lai cho giải pháp Chuyên gia Nguồn cung Ứng Bền vững này.

## Kết luận

Trợ lý Nguồn cung Ứng Bền vững minh họa cách AI tạo sinh, được cung cấp bởi Amazon Bedrock, có thể cách mạng hóa các quy trình mua sắm bằng cách tích hợp dữ liệu SAP với các chỉ số bền vững bên ngoài. Giải pháp khái niệm này chứng minh tiềm năng cho việc tự động hóa phân tích báo giá, nâng cao ra quyết định với các tiêu chí khách quan và đơn giản hóa tạo đơn đặt hàng mua—tất cả đồng thời duy trì bảo mật và khả năng mở rộng cấp doanh nghiệp.

Các tổ chức đang tìm cách hiện đại hóa các luồng công việc mua sắm SAP của họ có thể bắt đầu bằng cách khám phá các khả năng của Amazon Bedrock để xây dựng các agent AI tùy chỉnh. Kết hợp với các dịch vụ AWS như Amazon AppFlow cho tích hợp dữ liệu liền mạch và SAP BTP Integration Suite cho kết nối an toàn, cách tiếp cận này cung cấp một nền tảng linh hoạt cho đổi mới trên các chức năng kinh doanh khác nhau.

Chúng tôi khuyến khích bạn thử nghiệm với các công nghệ này trong môi trường của riêng bạn, thích ứng kiến trúc với nhu cầu cụ thể của bạn. Dù là nâng cao các sáng kiến bền vững hay tối ưu hóa các khía cạnh mua sắm khác, AI tạo sinh mang tiềm năng lớn cho việc thúc đẩy hiệu quả và giá trị chiến lược trong hoạt động của bạn.




