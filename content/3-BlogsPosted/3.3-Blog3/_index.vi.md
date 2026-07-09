---
title: "Blog 3"
date: 2026-06-28
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Bảo mật RAG đa người dùng với Amazon Bedrock và Verified Permissions

Các tổ chức lớn khi xây dựng ứng dụng Generative AI nội bộ thường đối mặt với một thách thức lặp đi lặp lại: kiểm soát quyền truy cập tài liệu của từng đội ngũ hoặc phòng ban mà không muốn nhân bản hạ tầng cho từng nhóm. Trong một tenant single-tenant, nhân viên phòng ban nào chỉ nên tiếp cận tài liệu phòng ban đó, trong khi cấp quản lý lại cần quyền truy cập chéo rộng hơn.

Retrieval-Augmented Generation (RAG) là giải pháp cân bằng giữa chi phí và hiệu năng. Bài viết này hướng dẫn cách sử dụng một instance Amazon Bedrock Knowledge Base (KB) duy nhất để giảm chi phí và độ phức tạp, đồng thời kết hợp với Amazon Verified Permissions (AVP) để cô lập dữ liệu động tại thời điểm truy xuất (runtime) nhờ các chính sách Cedar gọn nhẹ.

---

## Hướng dẫn kiến trúc

Điểm cốt lõi của kiến trúc này là chuyển đổi từ việc hard-code bộ lọc dữ liệu trong mã nguồn sang cơ chế đánh giá chính sách động bên ngoài. Thay vì xây dựng mỗi phòng ban một bộ Knowledge Base riêng biệt gây lãng phí tài nguyên, chúng ta gộp chung vào một KB duy nhất và kiểm soát thông qua các thẻ siêu dữ liệu (metadata tags) kết hợp với các điều kiện logic được thiết lập tập trung.

**Kiến trúc giải pháp bây giờ như sau:**

![Hình 1](/images/3-BlogsTranslated/3.3-Blog3/ARCHBLOG-1492-1.png)
> *Hình 1. Truy cập theo cấp độ vai trò (role) tới dữ liệu và tài nguyên dùng chung của tổ chức.*
![Hình 2](/images/3-BlogsTranslated/3.3-Blog3/ARCHBLOG-1492-2.png)
> *Hình 2. Pipeline ingestion: tài liệu upload lên Amazon S3 kích hoạt Amazon EventBridge, định tuyến qua Amazon SQS đến một AWS Lambda function ghi metadata. Một Lambda theo lịch sau đó kích hoạt job ingestion của Amazon Bedrock Knowledge Bases.*

![Hình 3](/images/3-BlogsTranslated/3.3-Blog3/ARCHBLOG-1492-3.png)
> *Hình 3. Luồng truy vấn: request của người dùng đi qua Amazon CloudFront và AWS WAF đến Amazon API Gateway, nơi Lambda Authorizer đánh giá phân quyền Lớp 1 (cấp API) với Verified Permissions. Nếu được phép, middleware Lambda đánh giá phân quyền Lớp 2 (cấp tài liệu), dựng metadata filter và gọi RetrieveAndGenerate.*
---

Giải pháp hướng tới mô hình phòng thủ theo chiều sâu (Defense-in-Depth) với 2 lớp độc lập:
- Lớp 1 (API Access): Một Lambda Authorizer tại API Gateway sẽ gọi Verified Permissions để quyết định người dùng có quyền gọi API hay không.
- Lớp 2 (Document Access): Một Middleware Lambda điều phối cuộc gọi đến Knowledge Base, tiếp tục truy vấn Verified Permissions để xác định các tag phòng ban nào được phép truy cập, từ đó xây dựng bộ lọc mã hóa tự động.

Mô hình này cung cấp sự cô lập ở mức logic (filter-level) trong nội bộ một doanh nghiệp, giúp thay đổi quy định phân quyền chỉ trong vài phút mà không cần redeploy lại code ứng dụng.
---

## Lựa chọn công nghệ và phạm vi giao tiếp

| Phạm vi giao tiếp                        | Các công nghệ / mô hình cần xem xét                                                        |
| ---------------------------------------- | ------------------------------------------------------------------------------------------ |
| Định danh & Claims nhóm người dùng                   | Amazon Cognito User Pools, JWT (trường cognito:groups)                               |
| Phòng vệ biên giới và giới hạn tần suất | AWS WAF, Amazon CloudFront |
| Đánh giá phân quyền 2 lớp (Lớp 1 & 2)                         | Amazon Verified Permissions (AVP), Cedar Policy Language  |
| Ingestion & Đóng gói dữ liệu tự động | Amazon EventBridge, Amazon SQS, AWS Lambda, Amazon S3 (Versioning & Object Lock) |
| Tìm kiếm vector và Sinh câu trả lời | Knowledge Bases for Amazon Bedrock, Amazon Titan Text Embeddings V2, Anthropic Claude 3 / Nova |
---

## Quy trình hoạt động của hệ thống (The Security Flow)

 ## 1. Ingestion Pipeline (Tự động gắn thẻ dữ liệu)
Để bộ lọc hoạt động chính xác lúc truy vấn, tài liệu phải được gán nhãn phòng ban ngay khi lưu trữ qua 2 giai đoạn:

- Giai đoạn 1 (Event-driven): Tài liệu upload lên S3 theo prefix (ví dụ docs/dept-a/) kích hoạt EventBridge tạo sự kiện, chuyển qua SQS buffer để gọi Lambda viết file sidecar .metadata.json tương ứng (ví dụ: department: dept-a).

- Giai đoạn 2 (Scheduled): Mỗi 5 phút, một Lambda kích hoạt StartIngestionJob để Bedrock quét S3, thực hiện chunking (300 tokens, 20% overlap), tạo embeddings và index vector kèm thuộc tính metadata vào cơ sở dữ liệu.

## 2. Query Flow (Luồng xử lý truy vấn bảo mật)
Khi người dùng gửi câu hỏi kèm theo ID Token, hệ thống thực hiện kiểm tra nghiêm ngặt:

- AWS WAF & API Gateway: Kiểm tra rate limit và tính hợp lệ của request.
- Lambda Authorizer (Layer 1): Giải mã JWT, kiểm tra cognito:groups, gọi AVP is_authorized để xác nhận quyền thực thi action query trên resource api. Nếu từ chối, trả về lỗi 403.
- Middleware Lambda (Layer 2): Tiếp tục gọi AVP để kiểm tra xem group của user được phép truy cập những KnowledgeBase ID nào (ví dụ: dept-a, dept-b).
- Xây dựng bộ lọc pre-filter: Chuyển đổi quyết định của AVP thành cấu trúc kb_filter (sử dụng toán tử equals hoặc orAll) rồi truyền vào API retrieve_and_generate của Bedrock.
- Bedrock KB & Guardrails: Cơ sở dữ liệu vector lọc bỏ các document không thuộc quyền sở hữu trước khi thực hiện tìm kiếm tương đồng. Mô hình LLM sinh câu trả lời dựa trên context sạch này, sau đó đi qua Guardrails for Amazon Bedrock để kiểm tra độ trung thực ngữ cảnh (contextual grounding) trước khi trả về user.
---

## Tính năng mới trong giải pháp

### 1. Định nghĩa chính sách phân quyền bằng Ngôn ngữ Cedar

Mọi logic phân quyền được tách biệt hoàn toàn khỏi code và lưu tại Verified Permissions. Ví dụ cấu hình chính sách Cedar cho phép nhóm dept-a truy vấn dữ liệu của chính họ và cấp quyền cho nhóm dept-c (Ban giám đốc) được quyền truy cập chéo (cross-department):
 // Quyền truy cập nội bộ của dept-a
permit(
    principal in GenAIApp::UserGroup::"dept-a",
    action == GenAIApp::Action::"query",
    resource == GenAIApp::KnowledgeBase::"dept-a"
);

// Quyền truy cập diện rộng của Ban giám đốc (dept-c)
permit(
    principal in GenAIApp::UserGroup::"dept-c",
    action == GenAIApp::Action::"query",
    resource
);


## 2. Thiết lập cơ chế Ingestion Safeguard trong Code
Đoạn mã Lambda kiểm tra nghiêm ngặt sự tồn tại của file sidecar metadata trước khi đẩy vào pipeline index, tránh việc document bị lọt vào hệ thống mà không có nhãn bảo mật:
# Xác thực sự hiện diện của file sidecar trước khi Ingestion
objects = s3.list_objects_v2(Bucket=BUCKET, Prefix=f"docs/{dept}/")
docs = [o["Key"] for o in objects.get("Contents", []) if not o["Key"].endswith(".metadata.json")]

for doc_key in docs:
    sidecar_key = f"{doc_key}.metadata.json"
    try:
        s3.head_object(Bucket=BUCKET, Key=sidecar_key)
    except s3.exceptions.ClientError:
        logger.warning(f"Bỏ qua {doc_key}: không tìm thấy file metadata sidecar")
        docs.remove(doc_key)
**Lưu ý bảo mật tầng lưu trữ:** Áp dụng S3 Bucket Policy để chỉ cho phép duy nhất ARN role của Tagging Lambda có quyền s3:PutObject đối với các file .metadata.json, ngăn chặn hành vi giả mạo hoặc thay đổi tag trái phép từ các user khác.

## Link bài viết gốc:
 https://aws.amazon.com/.../secure-multi-tenant-rag.../...