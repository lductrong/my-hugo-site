+++
title = "Kiểm tra các hàm"
date = 2021
weight = 6
chapter = false
pre = "<b>3.6 </b>"
+++

Trong task này, ta kiểm tra 2 hàm đã tạo có hoạt động hay chưa. Quy trình hoạt động của nó sẽ bao gồm:

1. Kích hoạt thủ công hàm Lambda New Post
1. Dữ liệu được thêm vào trong DynamoDB và gửi một tin nhắn đến chủ đề SNS
1. SNS kích hoạt hàm Chuyển đổi thành âm thanh
1. Hàm Chuyển đổi thành âm thanh sử dụng Amazon Polly để tạo tệp âm thanh
1. Tệp âm thanh sẽ được lưu vào trong bucket S3

## Thực hiện test:

#### 1. Kích hoạt thủ công hàm Lambda New Post

   Bước 1. Điều hướng đến trang dịch vụ của Lambda

   Bước 2. Trong menu bên trái chọn Functions 

   Chọn hàm PostReader\_NewPost

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.035.png)

   Bước 3. Chuyển sang tab Test sau đó chọn Test

   Thông hiện lên khi hàm thực thi thành công.

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.036.png)

#### 2. Kiểm tra dữ liệu có được thêm vào trong DynamoDB chưa
   Bước 1. Điều hướng đến trang dịch vụ của AWS DynamoDB

   Bước 2. Trong menu bên trái chọn Explore items

   Bước 3. Chọn bảng “posts”

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.037.png)

   Tùy vào số lần mà bạn đã test hàm gọi bài viết mới mà tồn tại số đối tượng bên trong bảng posts

#### 3. Kiểm tra sự hoạt động của hàm Chuyển đổi thành âm thanh

   Bước 1. Điều hướng tới trang dịch vụ của Lambda

   Bước 2. Trong menu bên trái chọn Functions và chọn hàm “ConvertToAudio”

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.038.png)

   Bước 3. Chọn tab Monitor

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.039.png)

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.040.png)

   Ta xem các biểu đồ giám sát việc gọi hàm. Như ở phần Recent invocations ta có thấy 2 lời gọi gần đây chính là 2 lần test hàm new post của chúng ta sau khi đã tạo hàm ConvertToAudio

#### 4. Kiểm tra xem đã có tệp âm thanh được tạo ra trong S3 hay chưa

   Bước 1. Điều hướng đến trang dịch vụ của S3

   Bước 2. Tìm và chọn bucket  audioposts-19012003 (tên bucket bạn đã đặt)

   Bước 3. Tại tab Objects ta thấy được các tệp âm thanh đã được tạo. Bây giờ ta có thể tick vào tệp, tải xuống và mở lên sẽ nghe được giọng của Polly Joanna nói “This is working!”

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.041.png)