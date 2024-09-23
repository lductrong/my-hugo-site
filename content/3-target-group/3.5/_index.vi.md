+++
title = "Tạo hàm Lambda “convert to audio”"
date = 2021
weight = 5
chapter = false
pre = "<b>3.5 </b>"
+++

Trong task này, chúng ta sẽ tạo một hàm Lambda để chuyển đổi văn bản được lưu trữ trong bảng DynamoDB thành tệp âm thanh. Đây là hàm trung tâm của ứng dụng quyết định chức năng chính của ứng dụng.
## <a name="_31hctgk31cnz"></a>Các bước để tạo hàm Lambda
Bước 1. Điều hướng vào AWS Management Console, tìm kiếm Lambda

Bước 2. Trong trang dịch vụ của Lambda chọn Create function

Bước 3. Cấu hình hàm

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.025.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.026.png)

- Chọn tùy chọn Author from scratch

- Function name: Đặt tên hàm:  ConvertToAudio

- Runtime: Chọn Python 3.12

- Nhấn vào Change default execution role để mở rộng các tùy chọn vai trò thực thi mặc định

    + Execution role: Chọn Use an existing role

    + Existing role: Chọn Lab-Lambda-Role

- Chọn create function để hoàn tất quá trình tạo hàm.

Bước 4. Thêm code vào hàm

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.027.png)

Trong phần Function code, xóa code hiện có

Dán code Python sau:
``` python
import boto3
import os
from contextlib import closing
from boto3.dynamodb.conditions import Key, Attr

def lambda_handler(event, context):

    postId = event["Records"][0]["Sns"]["Message"]

    print ("Text to Speech function. Post ID in DynamoDB: " + postId)

    # Retrieving information about the post from DynamoDB table
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])
    postItem = table.query(
        KeyConditionExpression=Key('id').eq(postId)
    )


    text = postItem["Items"][0]["text"]
    voice = postItem["Items"][0]["voice"]

    rest = text

    # Because single invocation of the polly synthesize_speech api can
    # transform text with about 3000 characters, we are dividing the
    # post into blocks of approximately 2500 characters.
    textBlocks = []
    while (len(rest) > 2600):
        begin = 0
        end = rest.find(".", 2500)

        if (end == -1):
            end = rest.find(" ", 2500)

        textBlock = rest[begin:end]
        rest = rest[end:]
        textBlocks.append(textBlock)
    textBlocks.append(rest)

    # For each block, invoke Polly API, which transforms text into audio
    polly = boto3.client('polly')
    for textBlock in textBlocks:
        response = polly.synthesize_speech(
            OutputFormat='mp3',
            Text = textBlock,
            VoiceId = voice
        )

        # Save the audio stream returned by Amazon Polly on Lambda's temp
        # directory. If there are multiple text blocks, the audio stream
        # is combined into a single file.
        if "AudioStream" in response:
            with closing(response["AudioStream"]) as stream:
                output = os.path.join("/tmp/", postId)
                if os.path.isfile(output):
                    mode = "ab"  # Append binary mode
                else:
                    mode = "wb"  # Write binary mode (create a new file)
                with open(output, mode) as file:
                    file.write(stream.read())

    s3 = boto3.client('s3')
    s3.upload_file('/tmp/' + postId,
      os.environ['BUCKET_NAME'],
      postId + ".mp3")
    s3.put_object_acl(ACL='public-read',
      Bucket=os.environ['BUCKET_NAME'],
      Key= postId + ".mp3")

    location = s3.get_bucket_location(Bucket=os.environ['BUCKET_NAME'])
    region = location['LocationConstraint']

    if region is None:
        url_beginning = "https://s3.amazonaws.com/"
    else:
        url_beginning = "https://s3-" + str(region) + ".amazonaws.com/"

    url = url_beginning \
            + str(os.environ['BUCKET_NAME']) \
            + "/" \
            + str(postId) \
            + ".mp3"

    # Updating the item in DynamoDB
    response = table.update_item(
        Key={'id':postId},
          UpdateExpression=
            "SET #statusAtt = :statusValue, #urlAtt = :urlValue",
          ExpressionAttributeValues=
            {':statusValue': 'UPDATED', ':urlValue': url},
        ExpressionAttributeNames=
          {'#statusAtt': 'status', '#urlAtt': 'url'},
    )

    return
```
Hàm Lambda thực hiện các nhiệm vụ sau:

+ Lấy nội dung văn bản từ DynamoDB dựa trên ID bài viết được cung cấp.
+ Chia văn bản thành các đoạn nhỏ hơn (nếu cần thiết).
+ Sử dụng Amazon Polly để chuyển đổi từng đoạn văn bản thành giọng nói.
+ Lưu các tệp âm thanh kết quả vào một bucket S3.

Việc chia nhỏ các đoạn văn bản để chuyển sang âm thanh sau đó nối các đoạn âm thanh lại với nhau giúp tránh được giới hạn 3000 chữ đầu vào của Polly. Điều này làm ứng dụng hoạt động thông suốt và hiệu quả.

Bước 5. Chọn Deploy để lưu những thay đổi

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.028.png)

Bước 6. Cấu hình biến môi trường

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.029.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.030.png)

Điều hướng đến tab “Configuration”

Trong bảng điều khiển bên trái, chọn “Environment variables”

Chọn vào “Edit” và thêm các biến sau:

+ Key: Nhập:  DB\_TABLE\_NAME, Value: Nhập: posts
+ Key: Nhập: BUCKET\_NAME, Value: audioposts-19012003 (Nhập tên bucket mà ta đã tạo trước đó)

Chọn vào “Save”

Bước 7. Cấu hình chung

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.031.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.032.png)

- Trong bảng điều khiển bên trái của tab Configuration, chọn “General configuration”

- Nhấp vào “Edit”

- Cập nhật Timeout thành 5 phút

- Nhấp vào “Save”

Bước 8. Cấu hình chức năng tự động kích hoạt khi có tin nhắn được gửi đến chủ đề SNS đã tạo trước đó.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.033.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.034.png)

- Trong bảng điều khiển bên trái của tab Configuration, chọn “Triggers”

- Nhấp vào “Add trigger”

- Chọn nguồn là: SNS

- SNS topics: Chọn new\_posts từ topics đã có sẵn

- Nhấp vào “Add”

Vậy là ta đã tạo xong 2 hàm Lambda. Kế đến ta có thể kiểm tra xem 2 hàm Lamda có giao tiếp thành công qua SNS hay không.