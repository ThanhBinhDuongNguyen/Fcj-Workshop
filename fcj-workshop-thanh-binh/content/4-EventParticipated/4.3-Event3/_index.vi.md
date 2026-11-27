---
title: "Event 3"
date: 2026-11-14
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Báo cáo tổng kết: “Cloud Mastery”

### Mục tiêu sự kiện

- Hiểu khoảng cách giữa một AI prototype và hệ thống AI doanh nghiệp sẵn sàng cho production, cùng vai trò của cloud trong quá trình này.
- Tìm hiểu Amazon Bedrock AgentCore và năm dịch vụ được giới thiệu: Runtime, Identity, Gateway, Memory và Observability.
- Theo dõi các use case thực tế và phần demo ứng dụng AI multi-agent nội bộ được xây dựng bằng Strands.
- Tìm hiểu các pattern multi-agent, agentic AI và cách RAG tạo câu trả lời dựa trên nguồn dữ liệu cụ thể.

### Diễn giả

- **Khang Duong** – Developer, thành viên FCAJ
- **Quynh** – Developer, thành viên FCAJ
- **Bao Nguyen** – Developer, thành viên FCAJ
- **Lam Anh** – Developer, thành viên FCAJ
- **Khuong Huynh** – Developer, thành viên FCAJ

### Các điểm nhấn chính

#### 1. Khoảng cách từ prototype đến production

- **Phép so sánh nhà hàng**: Prototype giống như căn bếp gia đình, AI doanh nghiệp giống chuỗi nhà hàng và cloud đóng vai trò bếp trung tâm.
- **AI trong doanh nghiệp**: AI là một tính năng trong sản phẩm theo luồng Data → AI Model → Feature → Users, chẳng hạn hệ thống gợi ý, chatbot hỗ trợ hoặc phát hiện gian lận.
- **AI học thuật và AI doanh nghiệp**: Hai hướng tiếp cận được so sánh theo mục tiêu, dữ liệu, người dùng và cách vận hành.
- **Bốn trụ cột của AI production**: Security, Reliability, Scalability và Cost.
- **Bảo mật AI**: Gồm quyền riêng tư dữ liệu, kiểm soát truy cập và các rủi ro đặc thù của AI.
- **Khả năng mở rộng trên AWS**: Có thể sử dụng Lambda, Auto Scaling và Load Balancer để điều chỉnh tài nguyên theo nhu cầu.
- **Vai trò của cloud**: Cung cấp các cơ chế bảo mật, khả năng mở rộng theo nhu cầu, độ tin cậy qua nhiều Availability Zone và mô hình chi phí pay-as-you-go.
- **Các công cụ AWS**: Lambda chạy code theo sự kiện mà không cần quản lý máy chủ; Bedrock cung cấp foundation model qua API; AgentCore hỗ trợ vận hành AI agent với Runtime, Memory và Identity.
- **Sử dụng model phù hợp**: Model lớn đảm nhiệm vai trò điều phối, còn các tác vụ chuyên biệt có thể sử dụng model nhỏ và tiết kiệm hơn.

#### 2. Use case thực tế và AgentCore Runtime

- **Use case**: Giới thiệu các ví dụ thực tế từ Cox Automotive và Mobileye.
- **Vị trí của AgentCore**: Kiến trúc gồm model, ứng dụng agent và Amazon Bedrock AgentCore.
- **Xây dựng agent bằng Strands**: Agent được phát triển với framework Strands.
- **Chạy agent trên AgentCore Runtime**: Phiên chia sẻ trình bày cách phát triển agent ở local, sau đó triển khai lên AgentCore Runtime để quản lý phiên làm việc.
- **Phân chia trách nhiệm**: Developer phụ trách business rule, dữ liệu, tool và kiểm thử; managed runtime đảm nhiệm compute, cô lập session và mở rộng tài nguyên.

#### 3. Identity, Gateway, Memory và Observability

- **Identity và Gateway**: AgentCore tách inbound access khỏi outbound access thông qua một luồng gồm năm bước; Gateway là một trong các dịch vụ được trình bày trong phần này.
- **Xây dựng context cho agent**: Context có thể gồm system prompt, tài liệu, tool, instruction, memory và lịch sử message.
- **Thách thức của agent memory**: Khả năng mở rộng lưu trữ, cập nhật memory, độ phức tạp tích hợp, quyền riêng tư, bảo mật, độ chính xác khi truy xuất và observability.
- **AgentCore Memory**: Gồm short-term memory cho message và session state; long-term memory cho semantic memory, sở thích người dùng, tóm tắt, episodic memory và custom strategy.
- **Luồng memory**: Agent gửi event, đọc memory record và sử dụng module tự động để trích xuất dữ liệu từ short-term memory sang long-term memory.
- **Observability – “You cannot fix what you cannot see”**: Hỗ trợ compliance, governance, traceability, lineage, theo dõi chi phí, troubleshooting, root cause analysis, phân tích latency, explainability và continuous evaluation.

#### 4. Demo và các pattern

- **Demo ứng dụng AI nội bộ**: User → FastAPI → Strands router; router chuyển từng request đến `qa_agent`, `ticket_agent` hoặc `research_agent`.
- **Các pattern multi-agent phổ biến**: Routing/Supervisor, Sequential, Parallel và Evaluator-optimizer.
- **Agentic pattern**: Reason → Act → Observe → Repeat hoặc Finish.
- **RAG – từ tài liệu đến câu trả lời có căn cứ**:
  - *Indexing*: Internal knowledge → embedding model → vector → vector store.
  - *Querying*: User question → query embedding → truy xuất top-K dữ liệu liên quan → bổ sung context vào prompt → tạo câu trả lời kèm nguồn.

### Bài học cốt lõi

#### Tư duy thiết kế

- **AI là một tính năng của sản phẩm**: AI production phải được thiết kế như một phần của sản phẩm phục vụ người dùng thật, không phải một model độc lập.
- **Ưu tiên bốn trụ cột**: Security, reliability, scalability và cost cần được xem xét ngay từ đầu.
- **Chọn model theo tác vụ**: Dùng model lớn cho orchestration và model nhỏ hơn cho các tác vụ chuyên biệt.

#### Kiến trúc kỹ thuật

- Triển khai agent trên **managed runtime** để developer tập trung vào business rule, dữ liệu, tool và kiểm thử, trong khi nền tảng xử lý compute, session isolation và scaling.
- Sử dụng **short-term memory và long-term memory** để duy trì context giữa các message và session.
- Bổ sung **observability** gồm traceability, cost visibility, root cause analysis và continuous evaluation trước khi đưa hệ thống vào production.
- Sử dụng **RAG** để tạo câu trả lời dựa trên dữ liệu nội bộ và có nguồn tham chiếu.

#### Chiến lược production

- Chọn pattern multi-agent phù hợp với tác vụ: Routing/Supervisor, Sequential, Parallel hoặc Evaluator-optimizer.
- Tách inbound access và outbound access để kiểm soát identity cùng permission của agent.

### Áp dụng vào công việc

- **Tìm hiểu sâu hơn về AI Security**: Phân tích rủi ro về dữ liệu, quyền truy cập và hành vi của AI trước khi lựa chọn thêm công cụ xử lý.
- **Thực hành luồng từ local đến ứng dụng**: Xây dựng một agent nhỏ bằng Strands và triển khai lên AgentCore Runtime.
- **Dùng bốn trụ cột làm checklist**: Rà soát security, reliability, scalability và cost cho các kiến trúc AWS của tôi.
- **Lập kế hoạch observability từ sớm**: Bổ sung logging, tracing và theo dõi chi phí trước khi triển khai production.

### Trải nghiệm sự kiện

Tham dự **Cloud Mastery** giúp tôi hiểu rõ hơn những yêu cầu cần thiết để đưa một ứng dụng AI từ prototype lên production trên AWS.

#### Học hỏi từ diễn giả

- Theo dõi ba phần chính: khoảng cách giữa prototype và production, use case thực tế với AgentCore Runtime, cùng các dịch vụ Identity, Gateway, Memory và Observability.
- Các phép so sánh với căn bếp gia đình, bếp trung tâm và đầu bếp giúp nội dung dễ hình dung hơn.

#### Tiếp cận kỹ thuật thực tế

- Theo dõi demo ứng dụng nội bộ sử dụng FastAPI và Strands router để chuyển request đến ba agent chuyên biệt.
- Hiểu cách các pattern multi-agent và RAG được kết hợp trong một ứng dụng hoạt động thực tế.

#### Bài học rút ra

- Một model chạy được chưa đủ; AI production cần security, reliability, scalability và cost control ngay từ đầu.
- Managed runtime giúp developer tập trung vào business rule, dữ liệu, tool và kiểm thử.
- Memory và observability là những thành phần quan trọng để agent có thể vận hành ổn định và truy vết được.
- Lựa chọn kích thước model theo tác vụ; có thể dùng model nhỏ hơn khi đáp ứng yêu cầu để tối ưu chi phí.

#### Nội dung cần tiếp tục tìm hiểu

- AI Security cần được nghiên cứu sâu hơn để hiểu đúng vấn đề trước khi lựa chọn công cụ hoặc giải pháp bảo vệ.

#### Một số hình ảnh sự kiện

![Bằng chứng tham dự Cloud Mastery](/images/4-Event/cloud-mastery.jpg)

> Nhìn chung, Cloud Mastery giúp tôi có góc nhìn tổng thể về agentic AI và hình dung rõ hơn quá trình đưa một AI agent từ môi trường local lên ứng dụng được triển khai thực tế.
