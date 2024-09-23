+++
title = "Tạo hàm Lambda Get Post"
date = 2021
weight = 7
chapter = false
pre = "<b>3.7 </b>"
+++

 Trong task này, chúng tạo hàm để lấy thông tin về các bài viết đã có sẵn trong DynamoDB

## Các bước tạo hàm:

   Bước 1. Điều hướng đến trang dịch vụ của Lambda

   Bước 2. Chọn Create function

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.042.png)

   Bước 3. Thiết lập cho hàm

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.043.png)

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.044.png)

   - Chọn tùy chọn Author from scratch

   - Function name: Đặt tên hàm:  PostReader\_GetPost

   - Runtime: Chọn Python 3.12

   - Nhấn vào Change default execution role để mở rộng các tùy chọn vai trò thực thi mặc định

        + Execution role: Chọn Use an existing role

        + Existing role: Chọn Lab-Lambda-Role

   - Chọn create function để hoàn tất quá trình tạo hàm.

   Bước 4. Thêm code vào hàm

   Xóa hết code sẵn có của hàm và chèn vào code python sau:
``` python
import boto3
import os
from boto3.dynamodb.conditions import Key, Attr

def lambda_handler(event, context):

    postId = event["postId"]

    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])

    if postId=="*":
        items = table.scan()
    else:
        items = table.query(
            KeyConditionExpression=Key('id').eq(postId)
        )

    return items["Items"]
```

   Bước 5. Chọn Deploy để lưu thay đổi.

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.045.png)

Và tương tự như 2 hàm đã tạo trước ta, bây giờ ta thêm tên của bảng DynamoDB vào biến môi trường cho hàm.

Bước 6. Cấu hình biến môi trường

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.046.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.047.png)

Điều hướng đến tab “Configuration”

Trong bảng điều khiển bên trái, chọn “Environment variables”

Chọn vào “Edit” và thêm biến sau:

+ Key: Nhập: DB\_TABLE\_NAME, Value: Nhập: posts

Chọn vào “Save”

Bước 7. Test hàm

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.048.png)

Chọn tab “Test”

Chọn Create new event

Event name: Nhập AllPosts

Event JSON:
```
{
    "postId": "\*"
}
```
Chọn Save sau đó chọn Test

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.049.png)

Một thông báo thực thi thành công được đưa ra, Chọn Details ta để xem chi tiết kết quả của test.