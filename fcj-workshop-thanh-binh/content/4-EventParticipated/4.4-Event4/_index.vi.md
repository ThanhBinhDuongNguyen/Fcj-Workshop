---
title: "Event 4"
date: 2026-11-21
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Báo cáo tổng kết: “AWS Student Community Day Vietnam 2026”

### Mục tiêu sự kiện

- Quy tụ cộng đồng sinh viên đam mê điện toán đám mây và AI tại Việt Nam.
- Giúp sinh viên tiếp cận trực tiếp kiến thức thực chiến từ chuyên gia và mở rộng kết nối trong kỷ nguyên Cloud & AI.
- Giới thiệu các AWS Student Builder Group của HUFLIT, HCMUTE, PTIT và SGU, bốn trường đồng tổ chức sự kiện.
- Tìm hiểu cách bảo vệ hệ thống đám mây bằng bảo mật mạng nhiều lớp trên AWS.

### Diễn giả

- **Kha Van** – Cloud Security Engineer
- **Trung Kiên** – Software Engineer, Core Team, Club HUFLIT
- **Nguyễn Hoàng Phúc** – Thành viên Core Team, AWS Student Builder Group HCMUTE
- **Hồ Đặng Thái Duy** – Thành viên Core Team, AWS Student Builder Group HCMUTE
- **Các diễn giả khác** – Thành viên Core Team của các AWS Student Builder Group tại HUFLIT, HCMUTE, PTIT và SGU

### Các điểm nhấn chính

#### 1. Cộng đồng AWS Student Builder

- **Sự kiện liên kết**: AWS Student Community Day Vietnam 2026 do liên minh các AWS Student Builder Group tại HUFLIT, HCMUTE, PTIT và SGU đồng tổ chức.
- **Kết nối cộng đồng**: Sự kiện dành phần lớn thời lượng cho hoạt động kết nối và giới thiệu hành trình xây dựng cộng đồng AWS Student Builder tại bốn trường.
- **Hành trình của Club HUFLIT**: Trung Kiên chia sẻ quá trình phát triển gần 3 năm, tương đương khoảng 1.000 ngày hoạt động của câu lạc bộ.
- **Hành trình của AWS Student Builder Group HCMUTE**: Hai thành viên Core Team chia sẻ quá trình xây dựng và phát triển nhóm.
- **Các cộng đồng được giới thiệu** (fanpage Facebook): AWS Student Builder Group Saigon University, AWS Student Builder Group at HUFLIT, AWS Student Builder Group PTIT HCM và AWS Student Builder Group HCMUTE.

#### 2. Bảo mật mạng trên Cloud (Phiên của anh Kha Van)

*Phiên chia sẻ trình bày kiến trúc bảo mật nhiều lớp và cách kiểm soát kết nối trên AWS, gồm phân vùng mạng, kiểm soát lưu lượng, phát hiện rủi ro và triển khai thực tế.*

- **Vì sao bảo mật mạng trên Cloud quan trọng**:
    - Nhiều tài khoản và VPC khiến ranh giới mạng phức tạp hơn.
    - Quyền truy cập, định tuyến và cấu hình quyết định mức độ phơi lộ.
    - Kết nối nội bộ quá rộng làm tăng nguy cơ tấn công lan rộng.
    - Tài nguyên thay đổi nhanh nên cần kiểm tra cấu hình thường xuyên.
    - Lưu lượng mã hóa và dịch vụ quản lý đòi hỏi cách giám sát phù hợp.
- **Kiến trúc tham chiếu: bảo vệ hệ thống theo nhiều lớp**:
    - *Lớp biên*: chống DDoS, bảo vệ website và kiểm soát truy cập từ Internet.
    - *Lớp mạng*: tách vùng mạng, định tuyến và chính sách phân đoạn.
    - *Lớp ứng dụng*: Security Group, chính sách mạng container và định danh.
    - *Lớp kiểm tra*: kiểm tra lưu lượng cần thiết ở lớp mạng và ứng dụng.
    - *Lớp giám sát*: network logs, phát hiện bất thường, kiểm tra kết nối và IaC.
- **Các dịch vụ bảo mật mạng có sẵn trên AWS**:
    - *Security Groups*: quy định kết nối nào được phép đến và đi từ tài nguyên trong VPC.
    - *Network ACLs*: lọc lưu lượng ở cấp subnet, có cả cho phép và từ chối.
    - *Route Tables / Transit Gateway (TGW)*: quyết định đường đi và kết nối giữa các vùng mạng.
    - *Network Firewall*: kiểm tra lưu lượng, lọc domain và phát hiện tấn công.
    - *WAF / Shield*: bảo vệ ứng dụng web và giảm thiểu tấn công DDoS.
    - *DNS Firewall*: kiểm soát yêu cầu phân giải tên miền qua DNS.
- **Kết nối nội bộ: ngăn tấn công lan giữa các hệ thống**:
    - *Trong cùng VPC*: dùng Security Groups để giới hạn kết nối giữa EC2 và ứng dụng. Lưu lượng trong cùng VPC không tự động đi qua firewall tập trung tại TGW.
    - *Giữa các VPC*: dùng Transit Gateway và route tables để phân vùng, chỉ đưa các luồng cần thiết qua firewall và kiểm tra đường đi hai chiều.
    - *Ưu tiên*: bảo vệ các kết nối giữa những vùng có mức độ tin cậy khác nhau.

#### 3. Prove It - Agentic Cloud Investigation Series (hiển thị trên slide)

- **Tổng quan thử thách**: Chuỗi thử thách do Cloud Thinker và AWS First Cloud Journey tổ chức, được University of Greenwich hỗ trợ, gồm hai vòng: *Find the Gaps* và *Find the Cause*.
- **Vòng 1 - quy tắc bằng chứng**: Với câu hỏi "What would you optimize in this AWS account for cost?", mỗi phát hiện phải nêu đúng tool call, dán toàn bộ raw output, gắn nhãn (Verified / Inferred / Assumed / Blocked), ghi nguồn, phạm vi và thời gian, và không được đề xuất xóa hoặc release như một hành động thực thi.
- **Báo cáo cuối gồm đúng ba phần**: Phạm vi đã kiểm tra; các đề xuất tối ưu được sắp xếp theo mức độ tác động, kèm nhãn, bằng chứng và raw output; cuối cùng là bước xử lý tiếp theo. Chỉ giữ các kết luận có đủ bằng chứng và che toàn bộ account ID cùng ARN.
- **Tình huống mẫu**: Một website tĩnh trên AWS báo *Access Denied* trong khi kết quả mong đợi là trang mở được. Các câu hỏi cần đặt ra là yêu cầu đi đến đâu, ai cần truy cập vào cái gì, và bản sửa đề xuất có cấp quá nhiều quyền hay không: hãy điều tra trước khi thay đổi quyền.
- **Vòng 2 - tìm nguyên nhân**: Tìm lỗi trong hạ tầng Kubernetes và đưa ra giải pháp, ví dụ một container liên tục bị OOMKilled và tự khởi động lại, hoặc pipeline orders-fulfillment bị kẹt do JSON lỗi tại offset 20.
- **Chuỗi nhân quả (Causal chain)**: Chuỗi các mắt xích logic nối liền từ sự thay đổi ban đầu cho đến tác động trực tiếp đến người dùng cuối: thay đổi cấu hình → cơ chế kỹ thuật → telemetry bất thường → service failure → customer impact.

### Bài học cốt lõi

#### Tư duy thiết kế

- **Phòng thủ nhiều lớp**: Bảo vệ hệ thống bằng nhiều lớp (biên, mạng, ứng dụng, kiểm tra và giám sát), không chỉ bằng một biện pháp.
- **Mức độ phơi lộ là kết quả của cấu hình**: Quyền truy cập, định tuyến và cấu hình quyết định mức độ phơi lộ của hệ thống, nên phải được thiết kế và kiểm tra có chủ đích.
- **Điều tra trước khi thay đổi**: Hiểu yêu cầu đi đến đâu và ai cần truy cập vào cái gì trước khi thay đổi quyền.

#### Kiến trúc kỹ thuật

- Dùng **Security Groups** trong VPC và **Transit Gateway cùng route tables** giữa các VPC để phân vùng mạng.
- Đặt **WAF / Shield** ở lớp biên và dùng **Network Firewall** và **DNS Firewall** để kiểm tra lưu lượng và yêu cầu DNS khi cần.
- Dùng **Network ACLs** cho quy tắc cho phép và từ chối ở cấp subnet.
- Duy trì **nhật ký mạng, cơ chế phát hiện bất thường và IaC** để cấu hình được kiểm tra thường xuyên.

#### Cách làm việc

- Ưu tiên bảo vệ các kết nối giữa những vùng có mức độ tin cậy khác nhau.
- Mỗi phát hiện phải có bằng chứng, nhãn (Verified / Inferred / Assumed / Blocked), nguồn, phạm vi và thời gian.

### Áp dụng vào công việc

- **Dùng năm lớp làm checklist**: Rà soát lớp biên, mạng, ứng dụng, kiểm tra và giám sát của các kiến trúc AWS của mình, chẳng hạn NexusOMS.
- **Rà soát quy tắc mạng**: Kiểm tra security group và định tuyến để chỉ cho phép các kết nối cần thiết, đặc biệt giữa public subnet và private subnet.
- **Tiếp tục tìm hiểu bảo mật**: Tiếp tục học sâu về bảo mật cloud và AI, nối tiếp các câu hỏi đặt ra sau Cloud Mastery.

### Trải nghiệm sự kiện

Tham dự **AWS Student Community Day Vietnam 2026** là cơ hội gặp gỡ các AWS Student Builder Group của bốn trường đại học. Các trải nghiệm nổi bật bao gồm:

#### Học hỏi từ các diễn giả
- Nghe cách các AWS Student Builder Group của HUFLIT và HCMUTE phát triển, trong đó có hành trình gần 3 năm của Club HUFLIT.
- Theo dõi phiên của anh Kha Van về kiến trúc bảo mật nhiều lớp, sự khác biệt giữa môi trường production và lab, cùng các cơ chế như Security Group và VPC Peering.

#### Cộng đồng và kết nối
- Các phần thảo luận giúp em hiểu thêm cách các AWS Student Builder Group tổ chức hoạt động và phát triển cộng đồng sinh viên.
- Biết thêm về bốn cộng đồng sinh viên và các fanpage của họ.

#### Bài học rút ra
- Bảo mật trên cloud được xây dựng theo nhiều lớp, từ lớp biên đến giám sát, chứ không dựa vào một công cụ duy nhất.
- Kết nối nội bộ quá rộng khiến tấn công dễ lan rộng, nên các kết nối giữa những vùng có mức độ tin cậy khác nhau cần được ưu tiên.
- Tài nguyên cloud thay đổi nhanh nên cần kiểm tra cấu hình thường xuyên.

#### Một số hình ảnh sự kiện
![Bằng chứng tham dự AWS Student Community Day Vietnam 2026](/images/4-Event/aws-student-community-day-2026.jpg)

> Nhìn chung, Community Day mang đến cơ hội kết nối với các AWS Student Builder Group, đồng thời cung cấp góc nhìn thực tế về kiến trúc bảo mật mạng nhiều lớp trên AWS.

