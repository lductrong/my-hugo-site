+++
title = "Xây dựng một ứng dụng Text-to-Speech serverless với Amazon Polly"
date = 2021
weight = 1
chapter = false
+++

# Xây dựng một ứng dụng Text-to-Speech serverless với Amazon Polly

## Tổng quan

Chuyển đổi văn bản thành giọng nói là một công nghệ khá phức tạp và cần có sự nghiên cứu chuyên sâu. Tình huống một số từ được đọc lên mà không có nghĩa hoàn toàn có thể xảy ra. Một số yếu tố gây nên những lỗi này của các ứng dụng chuyển đổi văn bản thành giọng nói bao gồm: **các từ có cách viết giống nhau nhưng cách phát âm khác nhau, chuẩn hóa văn bản, chuyển đổi văn bản thành âm vị, tên người, tiếng lóng và từ viết tắt.**

Amazon Polly cung cấp khả năng vượt qua các vấn đề trên, khi đó chúng ta chỉ cần tập trung vào việc xây dựng các ứng dụng sử dụng công nghệ TTS mà không cần lo lắng về các vấn đề giải thuật.
## Giới thiệu về Amazon Polly

Amazon Polly là dịch vụ chuyển đổi văn bản thành giọng nói (text-to-speech - TTS) của Amazon Web Services (AWS). Polly sử dụng công nghệ học sâu để chuyển văn bản thành giọng nói tự nhiên, đa ngôn ngữ, dễ dàng tích hợp vào các ứng dụng để tạo nội dung âm thanh chất lượng cao.

**Các điểm nổi bật của Amazon Polly:**

- **Giọng nói tự nhiên**: Polly hỗ trợ hàng chục giọng nói chân thực từ hơn 20 ngôn ngữ.
- **Phản hồi nhanh chóng**: Dịch vụ có thời gian phản hồi theo thời gian thực, phù hợp cho các ứng dụng như trợ lý ảo và chatbot.
- **Dễ dàng tích hợp**: Polly dễ dàng tích hợp thông qua API.
- **Hỗ trợ SSML**: Polly hỗ trợ Speech Synthesis Markup Language (SSML), cho phép tùy chỉnh giọng nói với các yếu tố như ngữ điệu, tốc độ và phát âm.
- **Chi phí hiệu quả**: Ta không bị tính phí bổ sung cho việc sử dụng âm thanh đã chuyển đổi.
- **Khả năng lưu trữ và phát lại**: Polly cho phép lưu trữ và phát lại các tệp âm thanh để sử dụng mà không cần kết nối mạng.

## <a name="_hwjuc0elk9u8"></a>Mục tiêu workshop
Ứng dụng Chuyển Đổi Văn Bản Thành Giọng Nói (Text-to-Speech - TTS) làm đa dạng thêm các cách tiếp cận thông tin, đặc biệt là đối với **người khiếm thị**. Cải thiện trải nghiệm người dùng trong các hệ thống trợ lý ảo, chatbot hoặc phục vụ những hoạt động giải trí như nghe sách, báo, truyện.

Trong workshop này, bạn sẽ được giới thiệu về cách Xây dựng một ứng dụng Text-to-Speech serverless với Amazon Polly. Cách mà Amazon Polly được tích hợp vào ứng dụng của mình để chuyển văn bản thành giọng nói tự nhiên.
