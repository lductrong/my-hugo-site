+++
title = "Tạo giao diện người dùng không máy chủ"
date = 2021
weight = 9
chapter = false
pre = "<b>3.9 </b>"
+++

Mặc dù ứng dụng đã hoạt động, nhưng hiện tại nó chỉ được cung cấp dưới dạng một RESTful web service. Để người dùng có thể tương tác dễ dàng hơn, chúng ta sẽ triển khai một trang web nhỏ được lưu trữ trên Amazon S3. Amazon S3 là một lựa chọn lý tưởng cho việc lưu trữ các trang web tĩnh vì nó đơn giản, chi phí thấp và dễ quản lý.

Trang web này sẽ sử dụng JavaScript để kết nối trực tiếp với API ta đã tạo ra, từ đó cung cấp tính năng chuyển đổi văn bản thành giọng nói (text-to-speech) thông qua giao diện web. Điều này giúp người dùng có thể nhập văn bản trực tiếp vào trang web và nghe kết quả giọng nói được tạo ra mà không cần phải thao tác với REST API trực tiếp.

## Các bước thực hiện:

Bước 1. Tải các file sau về máy, bạn có thể click chuột phải chọn “Save Link As..” hoặc mở file lên, chuột phải chọn “Save As..”

- [index.html](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/scripts/index.html)

- [scripts.js](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/scripts/scripts.js)

- [styles.css](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/scripts/styles.css)

{{% notice note %}}
Giữ nguyên tên và phần mở rộng của các file để hoạt động bình thường nhé!
{{% /notice %}}


Bước 2. Mở file scripts.js lên bằng một trình soạn thảo bất kỳ (Notepad chẳng hạn). Tại dòng đầu tiên bạn sẽ thấy: 
```
var API\_ENDPOINT = "YOUR\_API\_GATEWAY\_ENDPOINT"
```

Thay thế “YOUR\_API\_GATEWAY\_ENDPOINT” bằng URL Invoke của API bạn vừa triển khai ở trên. Sau đó nhớ lưu file lại nha.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.064.png)

Bước 3. Tạo 1 bucket S3 để chứa 3 file 

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.065.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.066.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.067.png)

- Điều hướng tới trang dịch vụ của S3.

- Chọn Create bucket và cấu hình các chi tiết sau:

- Bucket name: Vì tên cần là đọc nhất nên để không bị trùng ta sẽ đặt: www-BUCKET với BUCKET được thay bằng tên của bucket audioposts bạn tạo trước đó (VD: tên bucket của tôi là: www-audioposts-19012003)
{{% notice note %}}
Nhớ ghi lại tên của bucket để sử dụng sau này.
{{% /notice %}}



- Mục Object Ownership, chọn ACLs enabled

- Mục Block Public Access settings for this bucket: bỏ chọn Block all public access và đảm bảo rằng tất cả các tùy chọn khác cũng không được chọn.

- Một hộp cảnh báo sẽ xuất hiện, tick chọn mục “I acknowledge that the current settings might result in this bucket and the objects within becoming public”.

- Chọn Create bucket

Bước 4. Tải 3 file trên lên bucket Amazon S3

Sau khi bucket đã được tạo, chọn nó từ danh sách bucket ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.068.png)

Chọn Upload

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.069.png)

Chọn Add files và tải lên 3 file index.html, scripts.js và styles.css. Chọn Upoad để hoàn tất thao tác upload file.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.070.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.071.png)

{{% notice note %}}
Các file phải được để nguyên tên: index.html, scripts.js và styles.css
{{% /notice %}}

Bước 5. Quay lại trang bucket đang chứa 3 file, chuyển sang tab Permissions

Tại mục Bucket Policy và chọn nút Edit.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.072.png)

Dán Policy này vào trình chỉnh sửa:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::www-BUCKET/*"
            ]
        }
    ]
}
```
{{% notice note %}}
Thay thế www-BUCKET bằng tên của bucket của bạn.
{{% /notice %}}


Chọn Save changes.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.073.png)

Bước 6. Cuối cùng, ta sẽ kích hoạt lưu trữ trang web tĩnh, như vậy bucket sẽ hoạt động như một trang web tĩnh.

- Chuyển qua tab Properties.

- Tìm đến phần Static website hosting và chọn Edit.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.074.png)

- Chọn Enable cho Static website hosting

- Mục Index document: index.html

- Mục Error document - optional: index.html

{{% notice note %}}
Chúng ta đang sử dụng tệp index.html làm tài liệu lỗi.
{{% /notice %}}


Chọn Save changes.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.075.png)

Sau khi lưu, ta tìm lại về mục Static website hosting, sao chép Bucket website endpoint.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.076.png)


Và đó là tất cả! Bạn có thể kiểm tra xem trang web có hoạt động hay không.

Mở một tab trình duyệt web và dán URL Endpoint mà bạn vừa sao chép.

Web sẽ được mở lên:

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.077.png)

Nếu bạn viết gì đó vào ô văn bản và chọn Say it, new post sẽ được gửi đến ứng dụng của bạn. Ứng dụng sẽ chuyển đổi văn bản thành tệp âm thanh. 

Để xem các post và tệp âm thanh của chúng, nhập ID post hoặc \* (hiển thị tất cả) vào ô Tìm kiếm:

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.078.png)

Tùy thuộc vào kích thước của văn bản bạn cung cấp, mà quá trình chuyển đổi sang âm thanh có thể mất thời gian. Những post vẫn chưa chuyển đổi xong có Status là PROCESSING.

Nút Play để nghe âm thanh.

Link website: http://www-audioposts-19012003.s3-website-us-east-1.amazonaws.com/