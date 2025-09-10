---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Xây dựng storyboard nhân vật nhất quán với Amazon Nova trong Amazon Bedrock – Phần 1

Nghệ thuật dựng storyboard đóng vai trò nền tảng trong sáng tạo nội dung hiện đại, gắn liền với điện ảnh, hoạt hình, quảng cáo và thiết kế UX. Trước đây, các nhà sáng tạo thường dựa vào minh họa vẽ tay theo trình tự để phác thảo câu chuyện. Ngày nay, các mô hình nền tảng AI (Foundation Models - FMs) đang thay đổi toàn cảnh này. Các FM như [Amazon Nova Canvas](https://aws.amazon.com/ai/generative-ai/nova/creative/) and [Amazon Nova Reel](https://aws.amazon.com/ai/generative-ai/nova/creative/) mang đến khả năng biến văn bản và hình ảnh đầu vào thành hình ảnh và đoạn clip chất lượng chuyên nghiệp, hứa hẹn cách mạng hóa quy trình tiền sản xuất.

Tuy nhiên, bước tiến công nghệ này cũng đem đến những thách thức riêng. Mặc dù các mô hình này xuất sắc trong việc tạo ra nhiều ý tưởng nhanh chóng—một lợi thế cho quá trình khám phá sáng tạo—nhưng việc duy trì thiết kế nhân vật và phong cách thống nhất xuyên suốt các cảnh vẫn là một trở ngại lớn. Ngay cả những thay đổi nhỏ trong prompt hoặc cấu hình mô hình cũng có thể tạo ra kết quả thị giác khác biệt rõ rệt, gây gián đoạn tính liên tục của câu chuyện và phát sinh thêm khối lượng công việc cho người sáng tạo nội dung.

Để giải quyết những thách thức này, chúng tôi đã phát triển loạt bài hai phần nhằm khám phá các giải pháp thực tế giúp duy trì tính nhất quán thị giác. Trong Phần 1, chúng tôi đi sâu vào kỹ thuật thiết kế prompt và quy trình phát triển nhân vật, chia sẻ các mẫu prompt đã được kiểm chứng mang lại kết quả ổn định với Amazon Nova Canvas và Amazon Nova Reel. [Part 2](https://aws.amazon.com/blogs/machine-learning/build-character-consistent-storyboards-using-amazon-nova-in-amazon-bedrock-part-2/) sẽ trình bày các kỹ thuật như tinh chỉnh Amazon Nova Canvas để đạt độ nhất quán thị giác vượt trội và kiểm soát nhân vật chính xác.

<div style="display: flex; flex-wrap: wrap; gap: 8px; justify-content: center;">
  <img src="/images/3-BlogsTranslated/3.1-Blog1/image-1-1-300x169.jpg" alt="Girl running with llama">
  <img src="/images/3-BlogsTranslated/3.1-Blog1/image-2-2-300x169.jpg" alt="Girl running with llama">
  <img src="/images/3-BlogsTranslated/3.1-Blog1/image-3-300x169.jpg" alt="Girl running with llama">
</div>

### Thiết kế nhân vật nhất quán với Amazon Nova Canvas

Nền tảng của một storyboard hiệu quả bắt đầu từ việc thiết lập thiết kế nhân vật rõ ràng. Amazon Nova Canvas cung cấp nhiều kỹ thuật mạnh mẽ để tạo và duy trì sự nhất quán nhân vật xuyên suốt câu chuyện thị giác. Để giúp bạn áp dụng những kỹ thuật này trong dự án của mình, chúng tôi đã cung cấp ví dụ mã và tài nguyên trong [Github repository](https://github.com/aws-samples/sample-character-consistent-storyboard/tree/main/01-character-consistent-storyboarding-with-amazon-nova). . Bạn được khuyến khích theo dõi từng bước chi tiết cùng chúng tôi. Nếu bạn mới làm quen với Amazon Nova Canvas, chúng tôi khuyến nghị nên đọc trước [Generating images with Amazon Nova](https://docs.aws.amazon.com/nova/latest/userguide/image-generation.html) để nắm các khái niệm cơ bản.

### Prompt văn bản cơ bản

Amazon Nova Canvas biến mô tả văn bản thành hình ảnh trực quan. Khác với các mô hình ngôn ngữ lớn (LLMs), mô hình tạo ảnh không diễn giải lệnh hay suy luận – chúng phản hồi tốt nhất với mô tả chi tiết. Bao gồm những yếu tố cụ thể trong prompt, như đặc điểm ngoại hình, trang phục, và phong cách, sẽ ảnh hưởng trực tiếp đến kết quả tạo ra.

Ví dụ: *“Một cô bé 7 tuổi người Peru với mái tóc đen tết hai bím thấp, mặc đồng phục học sinh”* cung cấp các chi tiết rõ ràng để mô hình tạo ra hình ảnh nhân vật ban đầu, như minh họa dưới đây.

![Girl](/images/3-BlogsTranslated/3.1-Blog1/image-4-1-300x300.jpeg)
---

### Triển khai phong cách thị giác

Tính nhất quán trong storyboard không chỉ yêu cầu đặc điểm nhân vật mà còn cần phong cách thị giác thống nhất. Cách tiếp cận của chúng tôi tách thông tin phong cách trong prompt thành hai thành phần chính:

- **Mô tả phong cách** – Câu mở đầu định nghĩa chất liệu hình ảnh (ví dụ, *“Một minh họa theo phong cách tiểu thuyết đồ họa của”*)

- **Chi tiết phong cách** – Câu kết thúc nêu rõ yếu tố nghệ thuật (ví dụ, *“Đường nét đậm, bóng tối kịch tính, bảng màu phẳng”*)

Kỹ thuật có cấu trúc này cho phép khám phá nhiều phong cách nghệ thuật khác nhau, bao gồm tiểu thuyết đồ họa, phác thảo, và minh họa 3D, trong khi vẫn giữ sự nhất quán của nhân vật xuyên suốt storyboard. Dưới đây là mẫu prompt và thông tin phong cách bạn có thể thử nghiệm:

```yaml
{style_description} Một cô bé 7 tuổi người Peru với mái tóc đen tết hai bím thấp mặc đồng
phục học sinh. {style_details}
styles = [
    {
        "name": "graphic-novel",
        "description": "Một minh họa theo phong cách tiểu thuyết đồ họa của",
        "details": "Đường nét đậm, bóng tối kịch tính, bảng màu phẳng. Sử dụng ánh sáng
            tương phản mạnh và bố cục điện ảnh đặc trưng của tranh truyện. Bao gồm nét
            vẽ biểu cảm để truyền tải cảm xúc và chuyển động.",
    },
    {
        "name": "sketch",
        "description": "Một bản phác thảo đen trắng đơn giản của",
        "details": "Nét vẽ thô sơ, mang tính phác họa tạo thẩm mỹ storyboard. Tương phản
            cao. Không màu",
    },
    {
        "name": "digital-illustration",
        "description": "Một bản vẽ kỹ thuật số 3D của",
        "details": "Tương phản cao. Thiết kế nhân vật bo tròn. Kết xuất mượt. Chất liệu
            mềm mại. Ánh sáng lung linh",
    },
]
```

![Girl](/images/3-BlogsTranslated/3.1-Blog1/image-7-1.jpeg)

### Tích hợp cảnh với tham số nhất quán

Bây giờ chúng ta có thể kết hợp các kỹ thuật này để kiểm tra sự nhất quán của nhân vật trong các bối cảnh khác nhau, như minh họa dưới đây. Chúng ta giữ nguyên các tham số style, `seed`, and `cfgScale`, chỉ thay đổi phần mô tả cảnh để đảm bảo nhân vật vẫn dễ nhận diện xuyên suốt các chuỗi cảnh.

<div style="display: flex; flex-direction: column; gap: 24px; align-items: center; justify-content: center;">
  <div style="display: flex; flex-direction: row; gap: 24px; align-items: flex-start; max-width: 800px;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-8-288x300.jpg" alt="Girl" style="width: 288px; height: 300px;">
    <div>
     Một minh họa theo phong cách tiểu thuyết đồ họa của một cô bé 7 tuổi người Peru với mái tóc đen tết hai bím thấp mặc đồng phục học sinh <b>đạp xe trên đèo núi</b>. Đường nét đậm, bóng tối kịch tính, bảng màu phẳng. Sử dụng ánh sáng tương phản mạnh và bố cục điện ảnh đặc trưng của tranh truyện. Bao gồm nét vẽ biểu cảm để truyền tải cảm xúc và chuyển động.
    </div>
  </div>
  <div style="display: flex; flex-direction: row; gap: 24px; align-items: flex-start; max-width: 800px;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-9-288x300.jpg" alt="Girl" style="width: 288px; height: 300px;">
    <div>
     Một minh họa theo phong cách tiểu thuyết đồ họa của một cô bé 7 tuổi người Peru với mái tóc đen tết hai bím thấp mặc đồng phục học sinh <b>đi bộ trên con đường xuyên qua cỏ cao ở dãy Andes</b>. Đường nét đậm, bóng tối kịch tính, bảng màu phẳng. Sử dụng ánh sáng tương phản mạnh và bố cục điện ảnh đặc trưng của tranh truyện. Bao gồm nét vẽ biểu cảm để truyền tải cảm xúc và chuyển động.
    </div>
  </div>
  <div style="display: flex; flex-direction: row; gap: 24px; align-items: flex-start; max-width: 800px;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-10-288x300.jpg" alt="Girl" style="width: 288px; height: 300px;">
    <div>
      Một minh họa theo phong cách tiểu thuyết đồ họa của một cô bé 7 tuổi người Peru với mái tóc đen tết hai bím thấp mặc đồng phục học sinh <b>ăn kem trên bãi biển</b>. Đường nét đậm, bóng tối kịch tính, bảng màu phẳng. Sử dụng ánh sáng tương phản mạnh và bố cục điện ảnh đặc trưng của tranh truyện. Bao gồm nét vẽ biểu cảm để truyền tải cảm xúc và chuyển động.
    </div>
  </div>
</div>

### Quy trình phát triển storyboard

Dựa trên các kỹ thuật duy trì nhất quán nhân vật đã bàn, giờ đây chúng ta có thể triển khai một quy trình phát triển storyboard đầu cuối, biến mô tả nhân vật và cảnh viết thành storyboard trực quan mạch lạc. Cách tiếp cận hệ thống này sử dụng tham số cố định cho mô tả phong cách, `seed` và `cfgScale` để giữ nhân vật nhất quán trong khi thích ứng với nhiều ngữ cảnh khác nhau. Dưới đây là một số mô tả cảnh và nhân vật mẫu:

```yaml
"scenes":[
    {
        "description": "Mayu đứng ở rìa con đường núi, ôm chặt một cuốn sách. Mẹ cô,
            Maya, quỳ xuống bên cạnh, khích lệ và trao cho cô cuốn sách. Mayu trông lo
            lắng nhưng kiên định khi chuẩn bị bắt đầu hành trình."
    },
    {
        "description": "Mayu bắt gặp biển báo 'nguy hiểm' với hình vẽ con rắn. Cô bé
            trông sợ hãi, nhưng rồi nhớ lại lời mẹ dặn. Cô hít sâu, nhìn vào cuốn sách
            để lấy lại bình tĩnh, rồi tìm một cây gậy trên mặt đất."
    },
    {
        "description": "Mayu can đảm đi xuyên qua đám cỏ cao, vung gậy và tạo tiếng
            động để xua đuổi rắn. Khuôn mặt cô thể hiện sự pha trộn giữa sợ hãi và dũng
            khí khi tiến bước trên hành trình."
    }
],
"characters":{
    "Mayu":  "Một cô bé 7 tuổi người Peru với mái tóc đen tết hai bím thấp mặc đồng phục học sinh",
    "Maya":  "Một phụ nữ Peru lớn tuổi với mái tóc đen dài búi gọn, mặc trang phục truyền thống Peru"
}

```
![Diagram](/images/3-BlogsTranslated/3.1-Blog1/image-11-4.png)

Quy trình của chúng tôi sử dụng Amazon Nova Lite để trước tiên xây dựng các prompt hình ảnh tối ưu dựa trên các thực tiễn đã thiết lập, sau đó chuyển chúng cho Amazon Nova Canvas để tạo ảnh. Bằng cách đặt `numberOfImages` cao hơn (thường là ba biến thể), đồng thời duy trì `seed` và `cfgScale` cố định, chúng tôi cung cấp cho nhà sáng tạo nhiều lựa chọn vẫn giữ được tính nhất quán nhân vật. Prompt sử dụng cho Amazon Nova Lite như sau:

```yaml
Mô tả một hình ảnh thể hiện tốt nhất cảnh được mô tả. Dưới đây là một số ví dụ:
scene: Rosa ở trong bếp, lục lọi tủ đựng thức ăn để tìm món ăn nhẹ. Cô nghe thấy âm
    thanh lạ phát ra từ phía sau tủ và giật mình.
imagery: Một căn bếp ánh sáng mờ với kệ chất đầy đồ ăn, Rosa đang nhìn vào bên trong,
    khuôn mặt thể hiện sự tò mò xen lẫn sợ hãi.
scene: Rosa chào tạm biệt mẹ, Maya. Maya khích lệ cô và trao một món quà nhỏ được gói.
imagery: Cảnh toàn khuôn mặt Rosa đầy quyết tâm, đối diện Maya và nhận món quà nhỏ.
Chỉ mô tả hình ảnh. Dùng không quá 60 từ.
scene: {scene_description}
imagery:

```

Quy trình này đã tạo ra các khung storyboard như sau.

![Diagram](/images/3-BlogsTranslated/3.1-Blog1/1-1.png)

Mayu đứng ở rìa con đường núi, ôm chặt một cuốn sách. Mẹ cô, Maya, quỳ xuống bên cạnh, khích lệ và trao cho cô cuốn sách. Mayu trông lo lắng nhưng kiên định khi chuẩn bị bắt đầu hành trình.

![Diagram](/images/3-BlogsTranslated/3.1-Blog1/2-1.png)

Mayu bắt gặp biển báo "nguy hiểm" có hình vẽ con rắn. Cô bé trông sợ hãi, nhưng rồi nhớ lại lời mẹ. Cô hít một hơi sâu, nhìn vào cuốn sách để trấn tĩnh, rồi tìm một cây gậy trên mặt đất.

![Diagram](/images/3-BlogsTranslated/3.1-Blog1/3-1.png)
Mayu can đảm đi xuyên qua đám cỏ cao, vung gậy và tạo tiếng động để xua đuổi rắn. Khuôn mặt cô thể hiện sự pha trộn giữa sợ hãi và dũng khí khi tiến bước trên hành trình.

Mặc dù các kỹ thuật này cải thiện đáng kể sự nhất quán nhân vật, nhưng chưa hoàn hảo. Quan sát kỹ, bạn sẽ thấy ngay cả trong cùng một cảnh cũng có sự thay đổi về tính nhất quán nhân vật. Việc sử dụng seed cố định giúp kiểm soát các biến đổi này, và những kỹ thuật được trình bày trong bài viết này cải thiện đáng kể so với chỉ dùng prompt cơ bản. Tuy nhiên, nếu trường hợp của bạn yêu cầu sự nhất quán gần như hoàn hảo, chúng tôi khuyến nghị tiếp tục với [Part 2](https://aws.amazon.com/blogs/machine-learning/build-character-consistent-storyboards-using-amazon-nova-in-amazon-bedrock-part-2/), nơi chúng tôi khám phá các kỹ thuật tinh chỉnh nâng cao.

### Tạo video cho storyboard động

Nếu bạn muốn đi xa hơn hình ảnh tĩnh để biến storyboard thành các đoạn clip hoạt hình ngắn, bạn có thể sử dụng Amazon Nova Reel. Chúng tôi dùng Amazon Nova Lite để chuyển đổi prompt hình ảnh thành prompt video, thêm các chuyển động tinh tế và góc máy được tối ưu cho mô hình Amazon Nova Reel. Những prompt này, cùng với hình ảnh gốc, đóng vai trò ràng buộc sáng tạo để Amazon Nova Reel tạo ra các đoạn phim hoạt hình cuối cùng. Ví dụ về prompt và cảnh kết quả (dưới dạng GIF):

```yaml
Một con đường rừng ngập nắng với biển báo "Nguy hiểm" có hình con rắn. Một cô bé 7 tuổi
người Peru đứng đó, rõ ràng sợ hãi nhưng quyết tâm. Đường nét đậm, bóng tối kịch tính,
bảng màu phẳng. Ánh sáng tương phản mạnh và bố cục điện ảnh. Sương mờ trôi chậm. Camera
dolly tiến vào.

```

<div style="display: flex; flex-direction: row; gap: 16px; align-items: flex-start; justify-content: center;">
  <div style="text-align: center;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-15.jpeg" alt="Input image" width="300">
    <div>Input image</div>
  </div>
  <div style="text-align: center;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-16.gif" alt="Output video" width="300">
    <div>Output video</div>
  </div>
</div>

### Kết luận

Trong phần đầu tiên của loạt bài này, chúng tôi đã khám phá các kỹ thuật cơ bản để đạt được sự nhất quán về nhân vật và phong cách với Amazon Nova Canvas, từ kỹ thuật thiết kế prompt có cấu trúc cho đến xây dựng quy trình storyboard đầu cuối. Chúng tôi đã chứng minh rằng việc kết hợp mô tả phong cách, giá trị `seed`, và kiểm soát tham số `cfgScale` cẩn thận có thể cải thiện đáng kể sự nhất quán nhân vật trong các cảnh khác nhau. Chúng tôi cũng chỉ ra cách tích hợp Amazon Nova Lite với Amazon Nova Reel có thể nâng cao quy trình storyboard, vừa tạo prompt tối ưu vừa mang lại các đoạn hoạt hình minh họa.
