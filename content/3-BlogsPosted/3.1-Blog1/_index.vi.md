---
title: "Blog 1"
date: 2026-06-16
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Tự động hóa số hóa hồ sơ y tế với Amazon Bedrock Data Automation và AWS HealthLake

Trong khuôn khổ AWS Vietnam Community Day 2026, mình có cơ hội tìm hiểu về một giải pháp công nghệ rất thực tế và mang tính thời sự: ứng dụng Generative AI để tự động hóa quy trình số hóa tài liệu y tế ở quy mô doanh nghiệp.

Hiện nay, một thách thức lớn tại nhiều bệnh viện và cơ sở y tế là việc lưu trữ và xử lý một lượng khổng lồ hồ sơ bệnh án dưới dạng giấy tờ truyền thống, bản scan hoặc tệp tin PDF phi cấu trúc. Quy trình nhập liệu thủ công truyền thống không chỉ tiêu tốn nhiều thời gian, nhân lực mà còn tiềm ẩn tỷ lệ sai sót cao, gây khó khăn cho việc tra cứu, đồng bộ và phân tích dữ liệu lâm sàng để phục vụ điều trị.

Giải pháp được giới thiệu trong sơ đồ kiến trúc là sự kết hợp chặt chẽ giữa sức mạnh trích xuất của Amazon Bedrock Data Automation (BDA) và khả năng quản lý dữ liệu chuẩn hóa của AWS HealthLake, tạo nên một pipeline xử lý tự động hóa toàn diện từ đầu đến cuối.


## Hướng dẫn kiến trúc

Sơ đồ kiến trúc giải pháp mô tả một pipeline tự động, serverless và có khả năng mở rộng, giúp giảm khối lượng công việc thủ công và hạn chế sai sót cho các tổ chức y tế. Quy trình vận hành chi tiết bao gồm các bước tuần tự được thể hiện qua sơ đồ hệ thống dưới đây.

**Kiến trúc giải pháp bây giờ như sau**:

![Hình 1](/images/3-BlogsTranslated/3.1-Blog1/ARCHBLOG-1364-1.png)

> *Hình 1. Kiến trúc đầu-cuối thể hiện pipeline hướng sự kiện, từ lúc upload PDF đến khi lưu trữ dữ liệu tuân thủ FHIR.*


#### Luồng vận hành chi tiết của hệ thống


## Lựa chọn công nghệ và phạm vi giao tiếp

| Phạm vi giao tiếp                              | Các công nghệ / mô hình cần xem xét                                                        |
| ----------------------------------------       | ------------------------------------------------------------------------------------------ |
| Kích hoạt luồng dữ liệu tự động bên trong pipeline                   | Amazon S3 Event Notification, S3 Event Trigger                            |
| Xử lý logic và điều hướng dữ liệu trung gian | AWS Lambda (BDA Job Trigger, FHIR Processor) |
| Trích xuất dữ liệu thông minh và chuẩn hóa                         | Amazon Bedrock Data Automation, AWS |HealthLake                                      |
| Giám sát, ghi log và quản lý tập trung toàn hệ thống  |  Amazon CloudWatch for logs (Centralized logging)  |


## Phân rã dịch vụ và lưu trữ

Việc phân chia ranh giới xử lý giữa các thành phần giúp hệ thống hoạt động rời rạc và tăng tính bền vững: 
- Mỗi dịch vụ (S3, Lambda, Bedrock, HealthLake) chỉ tập trung giải quyết đúng một nhiệm vụ chuyên biệt.  
- Kết nối giữa các thành phần được thực hiện bất đồng bộ dựa trên sự kiện (Event-driven) sinh ra từ bộ lưu trữ Amazon S3.  
- Giảm thiểu hoàn toàn các kết nối đồng bộ trực tiếp giúp hệ thống tránh được tình trạng thắt nút cổ chai khi lượng hồ sơ tải lên tăng đột biến.


## Core processing (Bộ xử lý cốt lõi)

Cung cấp lớp xử lý dữ liệu nền tảng và tầng trích xuất thông minh, bao gồm:
- **Amazon S3** (input PDF) làm nơi tiếp nhận dữ liệu thô đầu vào.
- **Amazon Bedrock Data Automation kết hợp BDA Blueprints** (JSON templates) để bóc tách thông tin lâm sàng.
- **Amazon S3** (Extracted Medical Data) lưu trữ dữ liệu trung gian sau phân tích.

> Chỉ cho phép ghi dữ liệu thô trực tiếp vào S3 đầu vào, toàn bộ luồng xử lý và chuẩn hóa sau đó đều diễn ra gián tiếp qua AWS Lambda → đảm bảo tính toàn vẹn.



## Tầng chuẩn hóa dữ liệu đầu ra

- Cung cấp dịch vụ chuyên dụng AWS HealthLake để lưu trữ dữ liệu theo cấu trúc y tế quốc tế FHIR R4. 
- Đồng bộ dữ liệu thô thông qua tiến trình chuyển đổi tự động của hàm Lambda tạo tác vụ (FHIR Processor and Job Creator).  
- Loại bỏ hoàn toàn các kho chứa dữ liệu bị cô lập (Data Silos), giúp các ứng dụng client dễ dàng truy vấn qua cổng FHIR APIs.

---

---

## Tính năng mới trong giải pháp

### 1. Phân tích tài liệu thông minh với Generative AI

 Thay vì phải nhập từng dữ liệu bằng tay, hệ thống sử dụng Amazon Bedrock Data Automation kết hợp với các cấu trúc mẫu (Blueprints) để tự động hóa hoàn toàn việc chuyển đổi nội dung trong tài liệu thô thành dữ liệu có cấu trúc với độ chính xác cao.

## 2. Chuẩn hóa dữ liệu y tế chuyên sâu

Việc đồng bộ dữ liệu về kho lưu trữ AWS HealthLake theo chuẩn FHIR R4 giúp phá vỡ các rào cản cô lập dữ liệu thường gặp trong ngành y tế, hỗ trợ các tổ chức y tế khai thác, tìm kiếm và phân tích dữ liệu tốt hơn.

Đây là một ví dụ rõ ràng cho thấy Generative AI không chỉ dùng để tạo nội dung, mà còn có thể hỗ trợ xử lý dữ liệu thực tế trong những lĩnh vực quan trọng như chăm sóc sức khỏe.

## Link bài viết gốc:
https://aws.amazon.com/blogs/machine-learning/automate-medical-record-digitization-with-amazon-bedrock-data-automation-and-aws-healthlake/