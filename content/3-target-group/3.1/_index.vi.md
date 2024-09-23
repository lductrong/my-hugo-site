+++
title = "Tạo bảng DynamoDB"
date = 2021
weight = 1
chapter = false
pre = "<b>3.1 </b>"
+++

Trong task này, chúng ta sẽ tạo một bảng trong DynamoDB để lưu thông tin của các bài viết, bao gồm nội dung văn bản và URL của tệp MP3 tương ứng. Ta tạo một bảng “posts” có khóa chính (id) là một chuỗi do hàm New Post Lambda tạo ra khi  một new post được chèn vào cơ sở dữ liệu.

## Các bước thực hiện như sau:

Bước 1. Điều hướng tới trang dịch vụ của DynamoDB

Bước 2. Chọn Create table

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.006.png)

Bước 3. Thiết lập bảng như sau:

- Table name: posts

- Partition key: id

- Table settings: ta chọn mặc định Default settings

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.007.png)

Bước 4. Chọn Create table để tạo bảng