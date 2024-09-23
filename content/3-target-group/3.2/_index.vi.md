+++
title = "Tạo Amazon S3 Bucket"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2 </b>"
+++

Trong task này, chúng ta sẽ tạo một Amazon S3 bucket dùng làm nơi lưu trữ tất cả các tệp âm thanh sẽ được tạo bởi ứng dụng sau này. S3 là một lựa chọn rất lý tưởng với độ bảo mật, khả năng mở rộng cùng với độ bền cao (99.999999999%) và tính sẵn có cao (99.99%).

## Các bước để thực hiện:

Bước 1. Điều hướng vào AWS Management Console, tìm kiếm S3

Bước 2. Trong trang dịch vụ của S3 chọn Create bucket

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.008.png)

Bước 3. Cấu hình Bucket

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.009.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.010.png)

Bucket name: Nhập một tên độc nhất, ở đây tôi đặt là audioposts-19012003

{{% notice note %}}
Hãy ghi chú lại tên của bucket để có thể sử dụng về sau
{{% /notice %}}

Object Ownership: Chọn “ACLs enabled”.

Block Public Access settings:
- Bỏ chọn “Block all public access”.
- Sau đó bỏ chọn tất cả các tùy chọn khác.

Chọn vào ô “I acknowledge that the current settings might result in this bucket and the objects within becoming public.”

Bước 4. Chọn Create bucket để hoàn tất việc tạo bucket

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.011.png)

Bởi vì mỗi Amazon S3 bucket phải có một tên duy nhất trên toàn cầu trong tất cả các AWS. Trong trường hợp tên bị trùng, AWS sẽ báo lỗi “Bucket with the same name already exists” và tự động đưa bạn về mục bucket name để đặt một tên khác. Hãy thử đổi các con số cho đến khi tên không còn trùng nữa.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.012.png)

Ngược lại, sẽ có thông báo khi S3 bucket được tạo thành công. 