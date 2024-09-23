+++
title = "Thiết lập Môi trường"
date = 2020
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

## Khởi tạo một CloudFormation Stack

Chúng ta sẽ dùng một AWS CloudFormation template để thiết lập các tài nguyên cần dùng cho workshop trong AWS Region mà ta chọn. Điều này rất quan trọng vì các hướng dẫn tiếp theo sẽ phụ thuộc và các tài nguyên này. CloudFormation template này sẽ cung cấp các tài nguyên sau:

- IAM Role
- Bảng Amazon DynamoDB
- AWS Step Functions State Machine

Các bước thực hiện như sau:

Bước 1. Tải CloudFormation template (tệp YAML) và lưu vào một thư mực riêng trên máy tính: [Tải xuống tại đây](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/template/workshop-stack.yaml).

Bước 2. Hãy mở [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/) lên.

Bước 3. Chọn Create stack

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.002.png)

Bước 4. Tạo stack

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.003.png)

- Tại mục Prepare template chọn Choose an existing template
- Tại mục Template source chọn Upload a template file
- Chọn file template vừa tải về sau đó chọn Next.

Bước 5. Đặt tên cho stack

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.004.png)

Ở đây, tôi đặt tên là TTS-serverless. Chọn Next.

Bước 6. Các cấu hình của stack ta để mặc định

Tại mục Capabilities ta cho phép AWS CloudFormation được tạo tài nguyên IAM

Chọn Submit để triển khai template

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.005.png)