---
title: "Event 1"
date: 2026-05-24
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Bài thu hoạch “AWS Vietnam Community Day 2026”

### Mục Đích Của Sự Kiện

- Cập nhật các xu hướng mới về AI, GenAI, Agentic AI và Cloud Computing
- Chia sẻ kinh nghiệm triển khai hệ thống AI thực tế trong doanh nghiệp 
- Giới thiệu kiến trúc Multi-Agent và các giải pháp AWS phục vụ AI workloads
- Trao đổi về bảo mật, guardrails và compliance trong hệ thống AI hiện đại
- Kết nối cộng đồng developer, architect và sinh viên yêu thích công nghệ

### Danh Sách Diễn Giả

- **Pham Ng Hai Anh** – AWS Community Builder
- **Nguyen Tuan Thinh** – DevOps Engineer 
- **Tinh Truong** – Platform Engineer, GoTymeX 
- **Vy Lam** – Senior Business Systems Analyst, VPBank
- **Duc Dao** – Solution Architect, Cloud Kinetics
- Team Hackathon Project – UTMorpho 

### Nội Dung Nổi Bật

#### 1. Amazon Quick Suite – Agentic AI Assistant cho Doanh Nghiệp

**Diễn giả:** Pham Ng Hai Anh – AWS Community Builder

Trình bày giới thiệu **Amazon Quick** – nền tảng Agentic AI thống nhất giúp business user làm việc hiệu quả hơn. Thay vì phải thu thập thông tin thủ công từ nhiều nguồn, Amazon Quick cho phép:

- Kết nối hơn 40 data connectors, file upload và database
- Sử dụng Bedrock models, web search và hàng nghìn actions
- Tự động tạo MoM, gửi email, lên lịch họp, phân tích dữ liệu và xây dựng dashboard

#### 2. Amazon CloudFront – Foundation from Edge to Origin

**Diễn giả:** Nguyen Tuan Thinh – DevOps Engineer

Bài chia sẻ đi sâu vào vai trò của **Amazon CloudFront** như một nền tảng cốt lõi cho performance, security và cost optimization:

- Tiết kiệm chi phí mạnh về Data Transfer, Load Balancer và giảm tải EC2
- Bảo mật nâng cao với Origin Cloaking, VPC Private Origin, OAC, Mutual TLS, Signed URL và Geo-restriction
- Tăng hiệu năng với HTTP/3 (QUIC), multi-layer caching, Origin Shield và HTTP compression

#### 3. Context Is Everything – Làm AI Thực Sự Hiệu Quả

**Diễn giả:** Tinh Truong – Platform Engineer, GoTymeX

Đây là một trong những phần trình bày ấn tượng nhất về cách sử dụng AI hiệu quả.

Một số nội dung nổi bật:

- Nhấn mạnh rằng **Context quality quan trọng hơn Context quantity**
- Phân tích 3 sai lầm phổ biến khi prompt AI
- Giới thiệu framework:
  - Goal
  - Relevant Info
  - Constraints
  - Success Criteria
- Chia sẻ góc nhìn về tương lai của AI: từ Prompt → Context → Memory (Second AI Brain)

#### 4. Enterprise-Grade Multi-Agent System

Bài trình bày giới thiệu mô hình **Multi-Agent AI System** áp dụng cho bài toán **Startup Credit Scoring** trong lĩnh vực tài chính – ngân hàng.

##### Những vấn đề của hệ thống truyền thống

- Startup thường thiếu:
  - Credit history
  - Financial statements dài hạn
  - Collateral rõ ràng
- Dữ liệu thường phi cấu trúc, đa chiều và thay đổi nhanh

##### Multi-Agent Architecture

Hệ thống được chia thành nhiều agent chuyên biệt:

- Financial Analyst
- Market Analyst
- Team Evaluator
- Risk Assessor
- Compliance Agent

##### Lợi ích của Multi-Agent System

- Chuyên môn hóa theo từng domain
- Parallel processing
- Auditability và traceability tốt hơn
- Fault tolerance cao hơn single-agent
- Dễ mở rộng hệ thống

#### 5. Enterprise AI & Security + Non-Determinism của LLM

**Diễn giả:** Duc Dao và các diễn giả khác

Nội dung nhấn mạnh rằng AI trong doanh nghiệp không chỉ cần hoạt động tốt mà còn phải:

- Secure
- Reliable
- Scalable
- Compliant

Ngoài ra, phần trình bày cũng giải thích tính **non-deterministic** của LLM dù đã set `temperature = 0`.

##### Một số nguyên nhân chính

- Floating-point arithmetic trên GPU
- Parallel execution order
- Inference batching từ provider

##### Mitigation Strategies

- Structured outputs
- Majority voting
- Ensemble approach
- Testing kỹ lưỡng

#### 6. Hackathon Project – UTMorpho

Đây là phần chia sẻ về dự án AI UI Generator được phát triển trong hackathon LotusHacks.

##### Ý tưởng chính

Một AI UI Generator cho phép:

- Generate UI từ prompt
- Chỉnh sửa trực tiếp 
- Không cần re-prompt nhiều lần
- Giữ consistency giữa các lần chỉnh sửa

##### Các khó khăn gặp phải

- Token limits
- Burnout trong hackathon
- AI overgeneration
- Áp lực thời gian

##### Những bài học rút ra

- Team chemistry rất quan trọng
- Real frustration tạo ra real ideas
- AI nên được xem như teammate thay vì chỉ là tool

### Những Gì Học Được

#### Tư Duy Kiến Trúc

- Multi-agent systems phù hợp với các hệ thống enterprise phức tạp
- Context Engineering là kỹ năng quan trọng trong kỷ nguyên AI
- Security và compliance cần được thiết kế ngay từ đầu
- CloudFront là nền tảng vững chắc cho performance và cost optimization

#### Kiến Thức AI

- Hiểu rõ hơn về:
  - LLM inference
  - Non-determinism
  - Guardrails
  - Structured outputs


#### Kiến Thức Cloud & AWS

- Amazon Quick
- Bedrock Guardrails
- CloudFront
- Origin Shield
- OAC
- HTTP/3
- Edge Computing

#### Kỹ Năng Thực Tế

- Cách triển khai AI production systems
- Thiết kế scalable architecture
- Tư duy business-first và context-driven khi làm việc với AI

### Ứng Dụng Vào Công Việc Và Học Tập

- Áp dụng Context Framework khi sử dụng AI hỗ trợ lập trình và học tập
- Sử dụng Amazon CloudFront để tối ưu performance và chi phí cho project
- Thử xây dựng mini Multi-Agent system hoặc Personal Second AI Brain
- Tích hợp Bedrock Guardrails và structured output vào ứng dụng AI
- Áp dụng kiến trúc microservices, event-driven và domain-based vào project cá nhân

### Trải nghiệm trong event

Tham gia sự kiện **AWS Vietnam Community Day 2026** là một trải nghiệm rất bổ ích và truyền cảm hứng. Sự kiện không chỉ cập nhật các công nghệ mới liên quan đến AI và Cloud Computing mà còn có góc nhìn thực tế hơn về cách các doanh nghiệp triển khai AI ở quy mô production.

#### Học hỏi từ các chuyên gia

Các diễn giả đã chia sẻ rất nhiều kinh nghiệm thực tế liên quan đến:

- Multi-agent AI systems
- Agentic AI
- CloudFront foundational architecture
- Context Engineering
- Enterprise architecture
- AI security và compliance

#### Mở rộng kiến thức về AI

Tôi đặc biệt ấn tượng với các chủ đề:

- Non-determinism của LLM
- Context Is Everything
- Guardrails cho GenAI
- Agentic capabilities của Amazon Quick

#### Trải nghiệm cộng đồng công nghệ

Sự kiện quy tụ nhiều developer, architect và sinh viên yêu thích công nghệ, tạo nên môi trường trao đổi kiến thức rất năng động.

Thông qua networking và các bài chia sẻ, tôi học được:

- Cách tư duy system design thực tế
- Những khó khăn khi triển khai AI trong doanh nghiệp
- Kinh nghiệm làm hackathon và xây dựng MVP nhanh

#### Bài học rút ra

- Enterprise AI cần chú trọng security ngay từ đầu
- Context chất lượng cao là chìa khóa để khai thác sức mạnh AI
- Multi-agent systems phù hợp với các bài toán phức tạp
- Teamwork và communication đóng vai trò rất quan trọng


> Tổng thể, AWS Vietnam Community Day 2026 không chỉ giúp em học thêm nhiều kiến thức mới về AI và Cloud mà còn truyền động lực để em tiếp tục nghiên cứu và phát triển các project công nghệ trong tương lai.