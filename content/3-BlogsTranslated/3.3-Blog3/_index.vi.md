---
title: "Blog 3"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Xây dựng storyboard nhất quán về nhân vật với Amazon Nova trong Amazon Bedrock – Phần 2

Mặc dù việc xây dựng prompt cẩn thận có thể mang lại kết quả tốt, nhưng để đạt đến mức độ nhất quán chuyên nghiệp thường cần phải điều chỉnh bản thân mô hình nền tảng. Dựa trên kỹ thuật prompt engineering và phát triển nhân vật đã được đề cập trong [Phần 1](https://aws.amazon.com/blogs/machine-learning/build-character-consistent-storyboards-using-amazon-nova-in-amazon-bedrock-part-1/) của loạt bài hai phần, chúng ta sẽ nâng cao mức độ nhất quán cho các nhân vật cụ thể bằng cách tinh chỉnh mô hình nền tảng (FM) [Amazon Nova Canvas](https://aws.amazon.com/ai/generative-ai/nova/creative/) Thông qua kỹ thuật fine-tuning, người sáng tạo có thể hướng dẫn mô hình duy trì sự kiểm soát chính xác đối với diện mạo, biểu cảm và các yếu tố phong cách của nhân vật xuyên suốt nhiều cảnh.

Trong bài viết này, chúng ta sẽ lấy ví dụ từ bộ phim hoạt hình ngắn [Picchu](https://www.youtube.com/watch?v=vKSaF8NvMiQ), do FuzzyPixel thuộc Amazon Web Services (AWS) sản xuất, tiến hành chuẩn bị dữ liệu huấn luyện bằng cách trích xuất các khung hình nhân vật chính, và tinh chỉnh mô hình để duy trì tính nhất quán cho nhân vật Mayu và mẹ cô bé. Nhờ vậy, chúng ta có thể nhanh chóng tạo ra các ý tưởng storyboard cho những phần tiếp theo, như các hình ảnh dưới đây.

<div style="display: flex; flex-wrap: wrap; gap: 8px; justify-content: center;">
  <img src="/images/3-BlogsTranslated/3.3-Blog3/image-1-3-300x180.jpg" alt="Girl running with llama">
  <img src="/images/3-BlogsTranslated/3.3-Blog3/image-2-2-300x180.jpg" alt="Girl running with llama">
  <img src="/images/3-BlogsTranslated/3.3-Blog3/image-3-1-300x180.jpg" alt="Girl running with llama">
</div>

### Tổng quan giải pháp

Để triển khai một quy trình làm việc tự động, chúng tôi đề xuất kiến trúc giải pháp toàn diện sau, sử dụng các dịch vụ AWS để thực hiện từ đầu đến cuối.

![Diagram](/images/3-BlogsTranslated/3.3-Blog3/arch-1024x660.png)

Quy trình gồm các bước sau:

  1. Người dùng tải video nguồn lên một bucket [Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3) bucket.
  2. [Amazon Elastic Container Service](http://aws.amazon.com/ecs) (Amazon ECS) được kích hoạt để xử lý video.
Amazon ECS giảm tần suất khung hình, chọn các khung chứa nhân vật và cắt chính giữa để tạo ra hình ảnh nhân vật cuối cùng.
  3. Amazon ECS downsamples the frames, selects those containing the character, and then center-crops them to produce the final character images.
  4. Amazon ECS gọi mô hình [Amazon Nova](https://aws.amazon.com/ai/generative-ai/nova/) (Amazon Nova Pro) từ [Amazon Bedrock](https://aws.amazon.com/bedrock/) để tạo chú thích cho hình ảnh.
  5. Amazon ECS ghi chú thích hình ảnh và siêu dữ liệu vào S3 bucket.
  6. Người dùng sử dụng môi trường notebook trong [Amazon SageMaker AI](https://aws.amazon.com/sagemaker-ai) để gọi job huấn luyện mô hình.
  7. Người dùng tinh chỉnh mô hình Amazon Nova Canvas tùy chỉnh bằng cách gọi API ```create_model_customization_job``` và ```create_model_provisioned_throughput``` trong Amazon Bedrock, từ đó tạo ra mô hình tùy chỉnh sẵn sàng cho suy luận.

Quy trình được chia thành hai giai đoạn rõ ràng. Giai đoạn đầu (Bước 1–5) tập trung vào việc chuẩn bị dữ liệu huấn luyện. Trong bài viết này, chúng ta sẽ đi qua pipeline tự động trích xuất hình ảnh từ video đầu vào và tạo dữ liệu huấn luyện đã gắn nhãn. Giai đoạn thứ hai (Bước 6–7) tập trung vào tinh chỉnh mô hình Amazon Nova Canvas và thử nghiệm suy luận bằng mô hình đã được huấn luyện. Với các bước này, chúng tôi cung cấp dữ liệu hình ảnh đã được tiền xử lý và mã ví dụ đầy đủ trong [GitHub repository](https://github.com/aws-samples/sample-character-consistent-storyboard/tree/main/02-character-consistent-fine-tuning-with-amazon-nova-canvas) để bạn có thể dễ dàng thực hiện theo.

### Chuẩn bị dữ liệu huấn luyện

Bắt đầu với giai đoạn đầu tiên của quy trình, chúng ta xây dựng pipeline tự động để trích xuất nhân vật/đối tượng từ video, tạo hình ảnh có độ phân giải cao và nhãn chú thích chính xác thông qua các bước sau.

#### Trích xuất nhân vật sáng tạo

Trước tiên, chúng tôi khuyến nghị lấy mẫu khung hình ở khoảng thời gian cố định (ví dụ: 1 khung/giây). Sau đó, áp dụng [Amazon Rekognition](https://aws.amazon.com/rekognition/) để phát hiện nhãn và tìm kiếm khuôn mặt nhằm xác định khung hình và nhân vật quan trọng. Nhận diện nhãn có thể phân biệt hơn 2.000 nhãn khác nhau và xác định vị trí trong khung, rất hữu ích để phát hiện ban đầu các loại nhân vật chung hoặc phi nhân loại. Để phân biệt các nhân vật cụ thể, chúng ta dùng tính năng [search faces in a collection](https://docs.aws.amazon.com/rekognition/latest/dg/collections.html). Nếu hai cách tiếp cận trên chưa đủ chính xác, có thể dùng [Amazon Rekognition Custom Labels](https://aws.amazon.com/rekognition/custom-labels-features/) để huấn luyện mô hình phát hiện nhân vật cụ thể.

![Diagram](/images/3-BlogsTranslated/3.3-Blog3/image-5-1024x408.jpeg)

Sau khi phát hiện, mỗi nhân vật được cắt chính giữa với khoảng đệm điểm ảnh hợp lý. Tiếp theo, chạy thuật toán loại bỏ trùng lặp bằng mô hình [Amazon Titan Multimodal Embeddings](https://docs.aws.amazon.com/bedrock/latest/userguide/titan-multiemb-models.html) để loại bỏ hình ảnh tương tự vượt quá ngưỡng. Nhờ đó, bộ dữ liệu trở nên đa dạng hơn, tránh tình trạng quá khớp (overfitting). Ta có thể tinh chỉnh ngưỡng tương đồng để cân bằng giữa đa dạng dữ liệu và loại bỏ dư thừa.

#### Gắn nhãn dữ liệu

Chúng tôi tạo chú thích cho từng hình ảnh bằng cách sử dụng Amazon Nova Pro trong Amazon Bedrock, sau đó tải tệp kê khai hình ảnh và nhãn lên vị trí Amazon S3. Quá trình này tập trung vào hai khía cạnh quan trọng của kỹ thuật nhắc nhở: mô tả nhân vật để giúp FM xác định và đặt tên cho các nhân vật dựa trên các thuộc tính độc đáo của họ và tạo mô tả đa dạng để tránh các mẫu lặp đi lặp lại trong chú thích (ví dụ: "một nhân vật hoạt hình"). Sau đây là mẫu lời nhắc ví dụ được sử dụng trong quá trình ghi nhãn dữ liệu của chúng tôi:

```yaml
system_prompt = """ 
    Bạn là một chuyên gia mô tả hình ảnh chuyên nghiệp, người tạo ra alt ngắn gọn, tự nhiên văn bản giúp nội dung trực quan có thể truy cập được trong khi vẫn duy trì sự rõ ràng và tập trung. Nhiệm vụ của bạn là phân tích hình ảnh được cung cấp và cung cấp mô tả sáng tạo (20-30 từ) nhấn mạnh Ba nhân vật chính, nắm bắt được những điều cần thiết các yếu tố tương tác của chúng trong khi tránh các chi tiết không cần thiết.
"""

prompt = """ 

    1. Xác định các nhân vật chính trong ảnh: Nhân vật 1, Nhân vật 2 và Ký tự 3 ít nhất một ký tự sẽ có trong hình, vì vậy hãy cung cấp tối thiểu một mô tả với ít nhất một tên ký tự. 
      - "Nhân vật 1" mô tả nhân vật đầu tiên, đặc điểm chính, bối cảnh, thuộc tính. 
      - "Nhân vật 2" mô tả nhân vật thứ hai, đặc điểm chính, bối cảnh, thuộc tính. 
      - "Nhân vật 3" mô tả nhân vật thứ ba, đặc điểm chính, bối cảnh, thuộc tính. 
    2. Chỉ cần nêu tên của họ KHÔNG thêm bất kỳ đặc điểm tiêu chuẩn nào. 
    3. Chỉ chụp yếu tố hình ảnh bên ngoài các đặc điểm tiêu chuẩn 
    4. Nắm bắt sự tương tác cốt lõi giữa họ 
    5. Chỉ bao gồm các chi tiết theo ngữ cảnh rất quan trọng để hiểu cảnh 
    6. Tạo mô tả tự nhiên, trôi chảy bằng ngôn ngữ hàng ngày 
    
    Dưới đây là một số ví dụ
    
        ...
    
    
    [Xác định các nhân vật chính] 
    [Đánh giá sự tương tác chính của chúng] 
    [Lựa chọn các yếu tố ngữ cảnh quan trọng] 
    [Soạn thảo mô tả ngắn gọn, tự nhiên]
    
    
    {
        "alt_text": "[Mô tả ngắn gọn, tự nhiên tập trung vào các nhân vật chính]"
    }
    
    
    Lưu ý: Chỉ cung cấp đối tượng JSON làm phản hồi cuối cùng.
```

Đầu ra ghi nhãn dữ liệu được định dạng dưới dạng tệp JSONL, trong đó mỗi dòng ghép nối đường dẫn Amazon S3 tham chiếu hình ảnh với chú thích do Amazon Nova Pro tạo. Sau đó, tệp JSONL này được tải lên Amazon S3 để đào tạo. Sau đây là một ví dụ về tệp:

```yaml
{"image_ref": "s3://media-ip-dataset/characters/blue_character_01.jpg", "alt_text": "Nhân vật hoạt hình này có khuôn mặt tròn với đôi mắt to biểu cảm. Nhân vật có tông màu xanh lam đặc biệt với một búi tóc nhỏ ở trên. Thiết kế được cách điệu với các đường nét gọn gàng và cách tiếp cận tối giản đặc trưng của hoạt hình hiện đại."}
{"image_ref": "s3://media-ip-dataset/props/iconic_prop_series1.jpg", "alt_text": "Vật thể này dường như là một đạo cụ mang tính biểu tượng từ nhượng quyền thương mại. Nó có kim loại với các đường khắc đặc biệt và hình dạng độc đáo mà người hâm mộ sẽ nhận ra ngay lập tức. Ánh sáng làm nổi bật chất lượng kích thước và các chi tiết nhỏ giúp nhận biết ngay lập tức."}
```

#### Xác thực con người

Đối với các trường hợp sử dụng trong doanh nghiệp, chúng tôi khuyến nghị tích hợp quy trình có sự tham gia của con người (human-in-the-loop) để xác minh dữ liệu đã được gán nhãn trước khi tiến hành huấn luyện mô hình. Quá trình xác minh này có thể được triển khai bằng cách sử dụng [Amazon Augmented AI](https://aws.amazon.com/augmented-ai/) (Amazon A2I), một dịch vụ hỗ trợ người kiểm chứng (annotators) xác minh cả chất lượng hình ảnh lẫn chú thích. Để biết thêm chi tiết, hãy tham khảo tài liệu [Bắt đầu với Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-getting-started.html).

### Tinh chỉnh mô hình Amazon Nova Canvas

Sau khi đã có dữ liệu huấn luyện, chúng ta có thể tiến hành tinh chỉnh (fine-tune) mô hình Amazon Nova Canvas trong Amazon Bedrock. Amazon Bedrock yêu cầu một vai trò dịch vụ (IAM) [AWS Identity and Access Management](https://aws.amazon.com/iam/) để truy cập vào bucket S3 - nơi lưu trữ dữ liệu huấn luyện tùy chỉnh cho mô hình. Để biết thêm chi tiết, hãy xem tài liệu về [Quyền truy cập và bảo mật cho tùy chỉnh mô hình](https://docs.aws.amazon.com/bedrock/latest/userguide/custom-model-job-access-security.html). Bạn có thể thực hiện tác vụ tinh chỉnh trực tiếp thông qua bảng điều khiển Amazon Bedrock hoặc sử dụng API Boto3. Chúng tôi sẽ giải thích cả hai phương pháp trong bài viết này. Bạn có thể tìm thấy mã nguồn mẫu end-to-end trong file [picchu-finetuning.ipynb](https://github.com/aws-samples/sample-character-consistent-storyboard/blob/main/02-character-consistent-fine-tuning-with-amazon-nova-canvas/picchu-finetuning.ipynb).

#### Tạo job tinh chỉnh trên bảng điều khiển Amazon Bedrock

Bắt đầu bằng cách tạo job tinh chỉnh Amazon Nova Canvas trên bảng điều khiển Amazon Bedrock:

1. Trên bảng điều khiển Amazon Bedrock, trong ngăn điều hướng, chọn **Custom models** trong phần **Foundation models**.
2. Chọn **Customize model** và sau đó chọn **Create Fine-tuning job**.

![Diagram](/images/3-BlogsTranslated/3.3-Blog3/image-8-1024x453.jpg)

3. Trên trang **Create Fine-tuning job details**, chọn model muốn tùy chỉnh và nhập tên cho model đã được tinh chỉnh.
4. Trong phần **Job configuration**, nhập tên cho job và tùy chọn thêm tags để liên kết với nó.
5. Trong phần **Input data**, nhập vị trí Amazon S3 của file tập dữ liệu huấn luyện.
6. Trong phần **Hyperparameters**, nhập giá trị cho các hyperparameters, như thể hiện trong ảnh chụp màn hình sau.

![Diagram](/images/3-BlogsTranslated/3.3-Blog3/image-7-1.jpg)

7. Trong phần **Output data**, nhập vị trí Amazon S3 nơi Amazon Bedrock sẽ lưu đầu ra của job.
8. Chọn **Fine-tune model job** để bắt đầu quá trình tinh chỉnh.

Tổ hợp siêu tham số này đã mang lại kết quả tốt trong quá trình thử nghiệm của chúng tôi. Nhìn chung, việc tăng tốc độ học (learning rate) giúp mô hình huấn luyện mạnh mẽ hơn, nhưng thường tạo ra sự đánh đổi thú vị: chúng ta có thể đạt được tính nhất quán nhân vật nhanh hơn, nhưng có thể ảnh hưởng đến chất lượng hình ảnh tổng thể. Chúng tôi khuyến nghị tiếp cận có hệ thống khi điều chỉnh các siêu tham số. Hãy bắt đầu với kích thước batch và tốc độ học được đề xuất, sau đó thử tăng hoặc giảm số bước huấn luyện trước. Nếu mô hình gặp khó khăn trong việc học tập từ dữ liệu của bạn ngay cả sau 20.000 bước (mức tối đa được cho phép trong Amazon Bedrock), chúng tôi đề xuất tăng kích thước batch hoặc điều chỉnh tăng tốc độ học.

Những điều chỉnh này, dù tinh tế, có thể tạo ra sự khác biệt đáng kể trong hiệu suất của mô hình. Để biết thêm chi tiết về các siêu tham số, tham khảo [Hyperparameters for Creative Content Generation models](https://docs.aws.amazon.com/nova/latest/userguide/fine-tune-hyperparameters-content-generation-models.html).

#### Tạo job tinh chỉnh bằng Python SDK

Đoạn mã Python sau đây tạo job tinh chỉnh tương tự bằng cách sử dụng [API create_model_customization_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/bedrock/client/create_model_customization_job.html):

```Python
bedrock = boto3.client('bedrock')
jobName = "picchu-canvas-v0"
# Set parameters
hyperParameters = {
        "stepCount": "14000",
        "batchSize": "64",
        "learningRate": "0.000001",
    }

# Create job
response_ft = bedrock.create_model_customization_job(
    jobName=jobName,
    customModelName=jobName,
    roleArn=roleArn,
    baseModelIdentifier="amazon.nova-canvas-v1:0",
    hyperParameters=hyperParameters,
    trainingDataConfig={"s3Uri": training_path},
    outputDataConfig={"s3Uri": f"s3://{bucket}/{prefix}"}
)

jobArn = response_ft.get('jobArn')
print(jobArn)
```

When the job is complete, you can retrieve the new ```customModelARN``` using the following code:

```Python
custom_model_arn = bedrock.list_model_customization_jobs(
    nameContains=jobName
)["modelCustomizationJobSummaries"][0]["customModelArn"]
```

### Triển khai model trên bảng điều khiển Amazon Bedrock

Để triển khai model từ bảng điều khiển Amazon Bedrock, hoàn thành các bước sau:

1. Trên bảng điều khiển Amazon Bedrock, chọn **Custom models** trong phần **Foundation models** ở ngăn điều hướng
2. Chọn model tùy chỉnh mới và chọn **Purchase provisioned throughput**

![Diagram](/images/3-BlogsTranslated/3.3-Blog3/image-8-2-1024x453.jpg)

3. Trong phần **Provisioned Throughput details**, nhập tên cho provisioned throughput
4. Trong **Select model**, chọn model tùy chỉnh bạn vừa tạo
5. Sau đó chỉ định commitment term và model units

![Diagram](/images/3-BlogsTranslated/3.3-Blog3/image-9.jpg)

Sau khi mua provisioned throughput, một ARN (Amazon Resource Name) model mới sẽ được tạo. Bạn có thể gọi ARN này khi provisioned throughput đã vào hoạt động.

![Diagram](/images/3-BlogsTranslated/3.3-Blog3/image-10-1-1024x257.jpg)

#### Triển khai model bằng Python SDK

Đoạn mã Python sau đây tạo provisioned throughput bằng cách sử dụng [API create_provisioned_model_throughput:](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/bedrock/client/create_provisioned_model_throughput.html)

custom_model_name = "picchu-canvas-v0"

```Python
# Create the provision throughput job and retrieve the provisioned model id
provisioned_model_id = bedrock.create_provisioned_model_throughput(
    modelUnits=1,
    # create a name for your provisioned throughput model
    provisionedModelName=custom_model_name, 
    modelId=custom_model_arn
)['provisionedModelArn']
```

### Kiểm thử mô hình đã được tinh chỉnh
Khi provisioned throughput đã hoạt động, chúng ta có thể sử dụng đoạn mã sau để kiểm thử mô hình tùy chỉnh và thử nghiệm tạo một số hình ảnh mới cho phần tiếp theo của Picchu:

```Python
import json
import io
from PIL import Image
import base64

def decode_base64_image(img_b64):
    return Image.open(io.BytesIO(base64.b64decode(img_b64)))
    
def generate_image(prompt,
                   negative_prompt="text, ugly, blurry, distorted, low
                       quality, pixelated, watermark, text, deformed", 
                   num_of_images=3,
                   seed=1):
    """
    Generate an image using Amazon Nova Canvas.
    """

    image_gen_config = {
            "numberOfImages": num_of_images,
            "quality": "premium",
            "width": 1024,  # Maximum resolution 2048 x 2048
            "height": 1024,  # 1:1 ratio
            "cfgScale": 8.0,
            "seed": seed,
        }

    # Prepare the request body
    request_body = {
        "taskType": "TEXT_IMAGE",
        "textToImageParams": {
            "text": prompt,
            "negativeText": negative_prompt,  # List things to avoid
        },
        "imageGenerationConfig": image_gen_config
    } 

    response = bedrock_runtime.invoke_model(
        modelId=provisioned_model_id,
        body=json.dumps(request_body)
    )

    # Parse the response
    response_body = json.loads(response['body'].read())

    if "images" in response_body:
        # Extract the image
        return [decode_base64_image(img) for img in response_body['images']]
    else:
        return
seed = random.randint(1, 858993459)
print(f"seed: {seed}")

images = generate_image(prompt=prompt, seed=seed)
```

<div style="display: flex; flex-direction: row; gap: 16px; align-items: flex-start; justify-content: center;">
  <div style="text-align: left; width: 320px;">
    <img src="/images/3-BlogsTranslated/3.3-Blog3/image-11-300x300.jpg" alt="Input image" style="width: 300px; height: 300px; object-fit: cover; display: block; margin: 0 auto 8px auto;">
    <div>Khuôn mặt Mayu thể hiện sự pha trộn giữa lo lắng và quyết tâm. Mẹ quỳ bên cạnh, nhẹ nhàng ôm lấy em. Phong cảnh có thể nhìn thấy ở hậu cảnh.</div>
  </div>
  <div style="text-align: left; width: 320px;">
    <img src="/images/3-BlogsTranslated/3.3-Blog3/image-12-300x300.jpg" alt="Output video" style="width: 300px; height: 300px; object-fit: cover; display: block; margin: 0 auto 8px auto;">
    <div>Một vách đá dốc đứng với chiếc thang gỗ dài vươn xuống phía dưới. Mayu đang ở nửa chiếc thang với biểu cảm đầy quyết tâm trên khuôn mặt. Đôi bàn tay nhỏ của Mayu nắm chặt hai bên thang trong khi em cẩn thận đặt chân lên từng bậc. Môi trường xung quanh cho thấy một khung cảnh núi non hiểm trở.</div>
  </div>
  <div style="text-align: left; width: 320px;">
    <img src="/images/3-BlogsTranslated/3.3-Blog3/image-13-300x300.jpg" alt="Output video" style="width: 300px; height: 300px; object-fit: cover; display: block; margin: 0 auto 8px auto;">
    <div>Mayu đứng đầy tự hào trước cổng vào của một tòa nhà trường học đơn giản. Khuôn mặt em rạng rỡ với nụ cười tươi, thể hiện sự tự hào và thành tựu.</div>
  </div>
</div>

### Dọn dẹp

Để tránh phát sinh chi phí AWS sau khi hoàn thành việc kiểm thử, hãy thực hiện các bước dọn dẹp trong [picchu-finetuning.ipynb](https://github.com/aws-samples/sample-character-consistent-storyboard/blob/main/02-character-consistent-fine-tuning-with-amazon-nova-canvas/picchu-finetuning.ipynb) và xóa các tài nguyên sau:

- Miền Amazon SageMaker Studio
- Mô hình Amazon Nova đã tinh chỉnh và endpoint provisioned throughput

### Kết luận

Trong bài viết này, chúng tôi đã trình bày cách nâng cao tính nhất quán về nhân vật và phong cách trong storyboard từ [Phần 1](https://aws.amazon.com/blogs/machine-learning/build-character-consistent-storyboards-using-amazon-nova-in-amazon-bedrock-part-1/) bằng cách tinh chỉnh Amazon Nova Canvas trong Amazon Bedrock. Quy trình làm việc toàn diện của chúng tôi kết hợp xử lý video tự động, trích xuất nhân vật thông minh bằng Amazon Rekognition và tùy chỉnh mô hình chính xác bằng Amazon Bedrock để tạo ra một giải pháp duy trì độ trung thực về hình ảnh và tăng tốc đáng kể quá trình tạo storyboard. Bằng cách tinh chỉnh mô hình Amazon Nova Canvas trên các nhân vật và phong cách cụ thể, chúng tôi đã đạt được mức độ nhất quán vượt trội so với kỹ thuật prompt thông thường, giúp các nhóm sáng tạo có thể sản xuất storyboard chất lượng cao trong vài giờ thay vì vài tuần. Hãy bắt đầu thử nghiệm với tinh chỉnh Nova Canvas ngay hôm nay để bạn cũng có thể nâng tầm câu chuyện của mình với tính nhất quán tốt hơn về nhân vật và phong cách.
