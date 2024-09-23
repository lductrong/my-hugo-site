+++
title = "Tạo new post Lambda function"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4 </b>"
+++

Trong task này, chúng ta sẽ tạo hàm Lambda đầu tiên của ứng dụng. Hàm này chính là điểm vào, nhận thông tin về các bài viết mới cần được chuyển thành tệp âm thanh.

## Các bước để tạo hàm Lambda:

Bước 1. Điều hướng vào AWS Management Console, tìm kiếm Lambda

Bước 2. Trong trang dịch vụ của Lambda chọn Create function

Bước 3. Thiết lập cho hàm

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.016.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.017.png)

- Chọn tùy chọn Author from scratch

- Function name: Đặt tên hàm:  PostReader\_NewPost

- Runtime: Chọn Python 3.12

- Nhấn vào Change default execution role để mở rộng các tùy chọn vai trò thực thi mặc định

    - Execution role: Chọn Use an existing role

    - Existing role: Chọn Lab-Lambda-Role

- Chọn create function để hoàn tất quá trình tạo hàm.

Bước 4. Thêm code vào hàm

Trong phần Function code, xóa code hiện có

Dán code Python sau:
``` python
import boto3
import os
import uuid

def lambda_handler(event, context):

    recordId = str(uuid.uuid4())
    voice = event["voice"]
    text = event["text"]

    print('Generating new DynamoDB record, with ID: ' + recordId)
    print('Input Text: ' + text)
    print('Selected voice: ' + voice)

    # Tạo bản ghi mới trong bảng DynamoDB
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])
    table.put_item(
        Item={
            'id' : recordId,
            'text' : text,
            'voice' : voice,
            'status' : 'PROCESSING'
        }
    )

    # Gửi thông báo về bài viết mới tới SNS
    client = boto3.client('sns')
    client.publish(
        TopicArn = os.environ['SNS_TOPIC'],
        Message = recordId
    )

    return recordId
```

Hàm Lambda thực hiện các nhiệm vụ sau:

- Lấy hai tham số đầu vào:

    + Voice: Một trong số hàng chục giọng nói được hỗ trợ bởi Amazon Polly
    + Text: Nội dung của bài viết cần được chuyển đổi thành tệp âm thanh

- Tạo một bản ghi mới trong bảng DynamoDB với thông tin về bài viết mới

- Gửi thông tin về bài viết mới tới SNS

- Trả về ID của mục DynamoDB cho người dùng

Bước 5. Chọn deploy để lưu các thay đổi 

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.018.png)

Bước 6. Cấu hình biến môi trường

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.019.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.020.png)

Điều hướng đến tab “Configuration”

Trong bảng điều khiển bên trái, chọn “Environment variables”

Chọn vào “Edit” và thêm các biến sau:

+ Key: Nhập: SNS\_TOPIC, Value: [Dán ARN chủ đề SNS lúc trước ta tạo]
+ Key: Nhập: DB\_TABLE\_NAME, Value: Nhập: posts

Chọn vào “Save”

Bước 7. Cấu hình chung

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.021.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.022.png)

- Trong bảng điều khiển bên trái của tab Configuration, chọn “General configuration”

- Nhấp vào “Edit”

- Cập nhật Timeout thành 10 giây

- Nhấp vào “Save”

## Các bước kiểm tra hàm Lambda:

Bước 1. Tạo một sự kiện kiểm tra

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.023.png)

Điều hướng đến tab “Test”

Cấu hình một sự kiện kiểm tra mới với các chi tiết sau:

+ Event name: Joanna
+ Event JSON:
```
{
  "voice": "Joanna",
  "text": "This is working!"
}
```
+ Chọn Save

Bước 2. Chạy test

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.024.png)

Nhấp vào “Test” để thực thi sự kiện kiểm tra của bạn

Bạn sẽ thấy thông báo “Execution result: succeeded”

Mở rộng phần “Details” để xem nhật ký thực thi.