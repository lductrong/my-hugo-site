+++
title = "Công khai các Lambda function dưới dạng dịch vụ web RESTful"
date = 2021
weight = 8
chapter = false
pre = "<b>3.8 </b>"
+++

Như vậy là phần xử lý logic của chúng ta đã được thiết lập xong, bây giờ ta sẽ dùng Amazon API Gateway để công khai các hàm Lambda của chúng ta dưới dạng dịch vụ web RESTful. Điều này cho phép ta có thể gọi chúng bằng các giao thức HTTP một cách dễ dàng.

## Tạo API Gateway

Bước 1. Tại AWS Management Console tìm API Gateway

Bước 2. Tong các loại API ta sẽ dùng REST API, chọn vào Build

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.050.png)

Bước 3. Tạo REST API

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.051.png)

Chọn “New API”

API name: Đặt tên là: PostReaderAPI

Description: API cho ứng dụng TTS

API endpoint type: chọn Regional

Chọn Create API

## Cấu hình các phương thức HTTP

#### Phương thức POST

Bước 1. Trong bảng Resources, chọn tài nguyên gốc (/)

Bước 2. Chọn Create method

Bước 3. Cấu hình cho method

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.053.png)

- Method type: Chọn POST

- Integration type: Chọn hàm Lambda 

- Chọn hàm chứa PostReader\_NewPost

- Chọn Create method để hoàn tất quá trình tạo

#### Phương thức GET

Bước 1. Trong bảng Resources, chọn tài nguyên gốc (/)

Bước 2. Chọn Create method

Bước 3. Cấu hình cho method

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.054.png)

- Method type: Chọn GET

- Integration type: Chọn hàm Lambda 

- Chọn hàm chứa PostReader\_GetPost

- Chọn Create method để hoàn tất quá trình tạo
## <a name="_rphnw3flytm1"></a>Kích hoạt CORS
Cross-Origin Resource Sharing (CORS) cho phép gọi API từ các tên miền khác nhau.

Bước 1. Trong bảng Resources, chọn tài nguyên gốc (/)

Bước 2. Chọn Enable CORS

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.055.png)

Bước 3. Thiết lập cấu hình

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.056.png)

Gateway responses: Chọn “Default 4XX” và “Default 5XX”

Access-Control-Allow-Methods: Chọn GET và POST

Chọn Save để hoàn tất thiết lập.

## <a name="_jpehrfs77t3o"></a>Cấu hình tham số truy vấn
Tiếp theo, ta sẽ cấu hình phương thức **GET** để chấp nhận một **query parameter** (tham số truy vấn) có tên là **postId**. Tham số này sẽ cung cấp thông tin về **id của bài viết** mà bạn muốn lấy về.

Bước 1. Chọn phương thức GET

Bước 2. Trong phần Method request chọn Edit

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.057.png)

Bước 3. Chọn vào URL query string parameters để mở rộng nó ra

Bước 4. Chọn Add query string và tham số có Name: postId

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.058.png)

Bước 5. Chọn Save
## <a name="_5ud6upiakn7u"></a>Thiết lập Request Mapping

Hàm **PostReader\_GetPost Lambda** yêu cầu dữ liệu đầu vào phải ở định dạng **JSON**, vì vậy cần cấu hình API để chuyển đổi các tham số được người dùng gửi (chẳng hạn như **postId** từ query parameter) thành định dạng JSON trước khi chuyển đến hàm Lambda.

Bước 1. Chọn phương thức GET

Bước 2. Chuyển đến tab Integration Request chọn Edit

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.059.png)

Bước 3. Chỉnh lại các cài đặt

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.060.png)

- Mục Request body passthrough: chọn  “When there are no templates defined (recommended)”

- Nhấn vào mục Mapping Templates để mở rộng nó

- Content-Type: Nhập application/json

- Template body: Nhập:
```
{
    "postId" : "$input.params('postId')"
}
```
- Chọn Save để lưu thay đổi.

## <a name="_uhb4rxfrg3mn"></a>Triển khai API
Sau tất cả cài đặt, API đã sẵn sàng để triển khai rồi!

Bước 1. Chọn Deploy API

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.061.png)

Bước 2. 

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.062.png)

- Stage: Chọn \*New Stage\*

- Stage name: Đặt tên: Dev

- Phần mô tả có thể bỏ qua, chọn Deploy

Bước 3. Sau khi deploy thành công, hãy lưu lại Invoke URL để sử dụng nhé.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.063.png)