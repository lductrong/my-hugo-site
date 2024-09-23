+++
title = "Tạo một chủ đề SNS (SNS topic)"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3 </b>"
+++

Trong task này, chúng ta sẽ tạo một chủ đề SNS để tạo điều kiện giao tiếp giữa các hàm Lamda với nhau. Cần thiết lập chủ đề SNS là bởi quá trình chuyển đổi một bài viết ( dạng văn bản) thành tệp âm thanh được chia thành hai hàm AWS Lambda. Điều này bởi một vài lý do sau:

1. Tính bất đồng bộ và phản hồi nhanh cho người dùng:

Khi một new post được gửi, ta sẽ nhận ngay ID của bài viết trong DynamoDB dù cho quá trình chuyển đổi sang tệp âm thanh vẫn chưa hoàn tất. Điều này giúp cải thiện trải nghiệm người dùng, đặc biệt với những bài viết lớn, vì họ không phải chờ đợi lâu.

2. Khả năng mở rộng và tối ưu hóa tài nguyên:

Việc tách riêng hai quy trình cho phép tối ưu hóa việc sử dụng tài nguyên và mở rộng hệ thống khi cần. Ví dụ, hàm Lambda đầu tiên chỉ cần thực hiện các thao tác nhẹ nhàng như ghi nhận bài viết và lưu vào DynamoDB, trong khi hàm thứ hai sẽ xử lý chuyển đổi phức tạp hơn với Amazon Polly.

3. Tính linh hoạt:

Bằng cách tách biệt quá trình tạo bài viết và quá trình chuyển đổi âm thanh, chúng ta tạo ra một hệ thống linh hoạt và dễ bảo trì hơn.

## Các bước thực hiện:

Bước 1. Điều hướng vào AWS Management Console, tìm kiếm SNS

Bước 2. Trong trang dịch vụ của Amazon Simple Notification Service chọn mục Topics sau đó chọn Create topic

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.013.png)

Bước 3. Cấu hình topic

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.014.png)

- Type: Chọn Standard

- Name: Đặt tên là: new\_posts

- Display name: Đặt là: New Posts

Bước 4. Tại cuối trang, chọn Create topic

Bước 5. Lưu ARN của chủ đề

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.015.png)

Sau khi tạo, bạn sẽ thấy ARN của Chủ đề (Amazon Resource Name).
{{% notice note %}}
Ghi chú ARN này lại để sử dụng trong cấu hình Lambda functions.
{{% /notice %}}
