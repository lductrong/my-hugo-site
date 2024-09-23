+++
title = "Đề cương Hội thảo"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

## <a name="_v9ur3gv0eg5w"></a>Mục tiêu
## <a name="_b98hs9rl3p9h"></a>Sau khi hoàn thành workshop, bạn sẽ có thể:
- Tạo bảng Amazon DynamoDB để lưu trữ dữ liệu
- Tạo API RESTful với Amazon API Gateway
- Tạo các hàm AWS Lambda được kích hoạt bởi API Gateway
- Kết nối các hàm AWS Lambda với Amazon Simple Notification Service (SNS)
- Sử dụng Amazon Polly để tổng hợp giọng nói bằng nhiều ngôn ngữ và giọng điệu khác nhau
## <a name="_226ej8qlivw4"></a>Môi trường cho workshop
Vì là xây dựng một ứng dụng serverless, nghĩa là bạn không cần làm việc với các máy chủ - không cần chuẩn bị sẵn tài nguyên, vá lỗi, hoặc mở rộng. AWS Cloud sẽ tự động xử lý hết các điều trên, cho phép bạn tập trung vào ứng dụng của mình.

Ứng dụng này cung cấp hai methods chính – một để gửi thông tin về new post (post này sẽ được chuyển đổi thành tệp MP3) và một để lấy thông tin về một post (bài viết) đã có (bao gồm cả đường dẫn đến tệp MP3 được lưu trữ trong một S3 bucket của Amazon). Cả hai phương thức này đều được triển khai dưới dạng dịch vụ web RESTful thông qua **Amazon API Gateway**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.001.png)
#### <a name="_b8aslofboauv"></a>**Khi gửi thông tin về new post:**
1. **Thông tin** được gửi thông qua dịch vụ web RESTful, được Amazon API Gateway cung cấp. Dịch vụ này được gọi từ một trang web tĩnh được lưu trữ trên **Amazon S3**.
1. **Amazon API Gateway** kích hoạt một **AWS Lambda function** tên là "New Post", chịu trách nhiệm khởi tạo quá trình tạo tệp MP3.
1. **Lambda function** này chèn thông tin về bài viết vào một bảng **Amazon DynamoDB**, nơi lưu trữ thông tin về tất cả các bài viết.
1. Để xử lý toàn bộ quá trình một cách bất đồng bộ, ứng dụng sử dụng **Amazon Simple Notification Service (SNS)** để tách biệt quá trình nhận thông tin bài viết mới và quá trình chuyển đổi bài viết thành âm thanh.
1. Một **Lambda function** khác tên là "Convert to Audio" được đăng ký với chủ đề SNS này và sẽ được kích hoạt bất cứ khi nào có thông báo mới (đồng nghĩa với việc có một bài viết mới cần được chuyển đổi thành tệp âm thanh).
1. **Hàm Lambda “Convert to Audio”** sử dụng **Amazon Polly** để chuyển đổi văn bản thành tệp âm thanh theo ngôn ngữ được chỉ định (nó phải cùng với ngôn ngữ của văn bản).
1. **Tệp MP3 mới** được lưu vào một **S3 bucket**.
1. Thông tin về bài viết được cập nhật trong bảng **DynamoDB**, bao gồm cả **URL của tệp âm thanh** đã được lưu trong S3.
   #### <a name="_ckkjfkkadct9"></a>**Khi lấy thông tin về post:**
1. Dịch vụ web RESTful được triển khai thông qua **Amazon API Gateway**, cung cấp phương thức lấy thông tin bài viết. Phương thức này chứa văn bản của bài viết và đường dẫn tới **S3 bucket** nơi lưu trữ tệp MP3. Dịch vụ web này cũng được gọi từ một trang web tĩnh lưu trữ trên **Amazon S3**.
1. **Amazon API Gateway** gọi hàm **Get Post Lambda**, triển khai logic để lấy dữ liệu bài viết.
1. **Get Post Lambda function** truy xuất thông tin về bài viết (bao gồm cả đường dẫn đến **Amazon S3**) từ bảng **DynamoDB** và trả về thông tin đó.