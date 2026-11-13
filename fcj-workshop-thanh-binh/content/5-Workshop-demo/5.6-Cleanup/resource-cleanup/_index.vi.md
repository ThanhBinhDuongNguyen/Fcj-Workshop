---
title: "Xóa toàn bộ tài nguyên"
date: 2026-11-13
weight: 1
chapter: false
---

Thực hiện đúng thứ tự phụ thuộc. Xác nhận NAT Gateway, Elastic IP, ALB, ECS service và endpoint đã bị xóa vì đây là các tài nguyên dễ tiếp tục phát sinh phí.

## 17. Cleanup sau demo

Cleanup chỉ sau khi đã lưu ảnh báo cáo và xác nhận không cần website hoạt động. Supabase project không bị xóa trong bước này.

## 17.1. Tắt request public trước

1\. CloudFront → distribution → **Disable**; chờ status Deployed. Sau khi disabled, CloudFront không còn phục vụ website.

2\. ECS → Cluster → Service → **Update** desired count 0; chờ không còn Running tasks.

3\. Xóa ECS service; sau đó deregister các Task Definition revision nếu cần.

4\. EC2 → Load Balancers: xóa ALB; EC2 → Target Groups: xóa target group sau khi ALB không còn tham chiếu.

## 17.2. Xóa resource có phụ thuộc

Thứ tự quan trọng để tránh AWS từ chối xóa:

1\. Xóa CloudFront distribution sau khi đã disable/deployed. Nếu báo origin/behavior reference, xóa behavior API trước khi xóa ALB origin; cuối cùng xóa distribution.

2\. S3: bucket frontend → xóa hết versions/delete markers nếu versioning bật → xóa bucket. Lệnh aws s3 rm --recursive không xóa các object version cũ trong bucket versioned; dùng Empty bucket trong Console để xóa mọi version.

3\. ECR: xóa repository nexusoms-demo-api, chọn xóa toàn bộ images khi Console hỏi.

4\. Secrets Manager: chọn nexusoms/demo/application → Schedule deletion. Nếu cần xóa ngay cho demo, bật force delete chỉ khi bạn chắc chắn không còn task cần secret.

5\. CloudWatch: xóa log group /ecs/nexusoms-demo-api sau khi đã tải/lưu log cần thiết.

6\. VPC → Endpoints: xóa S3 Gateway Endpoint duy nhất. Bản này không tạo bốn Interface Endpoint.

7\. NAT Gateways: xóa NAT A và B; chờ Deleted.

8\. Elastic IPs: release hai EIP NAT sau khi NAT đã xóa; kiểm tra không còn gắn resource khác.

9\. Route tables: xóa route private-a/private-b và public route table sau khi xóa association; route table main mặc định không cần xóa.

10\. Subnets: xóa bốn subnet.

11\. IGW: detach khỏi VPC, sau đó delete.

12\. Security Groups do bạn tạo: xóa ALB SG và task SG. Nếu SG còn được resource nào dùng, xóa resource đó trước.

13\. Xóa VPC cuối cùng.

14\. CloudTrail: nếu trail/bucket được tạo riêng cho demo, tắt trail trước; chỉ xóa audit bucket sau khi đã lưu evidence và xác nhận không còn cần. Bucket có versioning phải được làm rỗng hoàn toàn trước khi xóa.

15\. IAM → Users → nexusoms-demo-deployer: Deactivate rồi xóa Access Key; gỡ policy AdministratorAccess; sau đó xóa IAM user nếu không còn dùng. Trên máy local, chạy aws configure list-profiles; chỉ xóa block \[profile nexusoms-demo\] trong %USERPROFILE%\\.aws\\config và block \[nexusoms-demo\] trong %USERPROFILE%\\.aws\\credentials. Không xóa cả hai file nếu còn profile khác.

VPC và NAT Gateway là phần dễ tiếp tục tạo chi phí; sau cleanup mở **VPC → NAT Gateways** xác nhận không còn NAT. Mở EC2 → Elastic IPs để kiểm tra không còn EIP chưa dùng; Billing → Bills/Cost Explorer để kiểm tra các dịch vụ khác.

Ảnh cần chụp: 11-cleanup/cleanup-complete.png sau khi lưu bằng chứng và xác nhận các resource chính đã dừng/xóa.

## 20. Cleanup toàn bộ tài nguyên AWS

Thực hiện cleanup theo đúng thứ tự dưới đây. Không xóa VPC trước vì các dịch vụ khác còn giữ network interface trong VPC.

Region chính:

```
Asia Pacific (Singapore) - ap-southeast-1
```

CloudFront và WAF sử dụng phạm vi Global.

## 1. CloudFront và WAF

1. Vào CloudFront → Distributions.
2. Mở từng distribution NexusOMS.
3. Nếu có WAF, vào Security → Edit và gỡ WAF.
4. Chọn Disable.
5. Chờ distribution cập nhật xong.
6. Chọn Delete.

Sau đó:

- Xóa CloudFront Function dùng để rewrite SPA.
- Xóa Origin Access Control của bucket NexusOMS.
- Vào WAF & Shield, chọn phạm vi Global và xóa Web ACL NexusOMS nếu còn.

## 2. ECS Service

Nếu ECS Console đã tự tạo CloudFormation stack:

1. Vào CloudFormation → Stacks.
2. Chọn stack có tên bắt đầu bằng:

```
ECS-Console-V2-Service-nexusoms-demo...
```

3. Kiểm tra tab Resources có ECS Service.
4. Chọn Delete stack.
5. Chờ DELETE_COMPLETE.

Kiểm tra lại tại:

```
ECS → Clusters → nexusoms-demo-cluster
```

Kết quả phải là:

- Services: 0
- Running tasks: 0
- Pending tasks: 0

## 3. Application Load Balancer

1. EC2 → Load Balancers.
2. Xóa nexusoms-demo-alb.
3. Nếu bị chặn, tắt Deletion protection trong Attributes.
4. Vào EC2 → Target Groups.
5. Xóa nexusoms-demo-api-tg.

## 4. NAT Gateway và Elastic IP

6. VPC → NAT gateways.
7. Xóa cả hai NAT Gateway.
8. Chờ trạng thái Deleted.
9. Vào EC2 → Elastic IP addresses.
10. Release hai Elastic IP từng gắn với NAT Gateway.

Xóa NAT Gateway không tự giải phóng Elastic IP.

## 5. VPC Endpoint

Vào VPC → Endpoints, lọc theo nexusoms-demo-vpc và xóa nếu còn:

- ecr.api
- ecr.dkr
- logs
- secretsmanager
- s3

Interface Endpoint tính phí theo giờ nên cần kiểm tra kỹ phần này.

## 6. ECS Cluster, Task Definition và ECR

- Xóa nexusoms-demo-cluster.
- Deregister các revision thuộc task definition nexusoms-demo-api.
- Vào ECR → Private repositories.
- Xóa repository nexusoms-demo-api cùng toàn bộ image.

## 7. Secrets Manager

1. Vào Secrets Manager.
2. Chọn nexusoms/demo/application.
3. Actions → Delete secret.
4. Chọn recovery window 7 ngày.
5. Schedule deletion.

Secret được lên lịch xóa sẽ ngừng tính phí.

## 8. CloudWatch và CloudTrail

Trong CloudWatch, xóa:

- Log group /ecs/nexusoms-demo-api
- Alarms NexusOMS
- Dashboard NexusOMS
- Các log group riêng của dự án

Nếu đã tạo CloudTrail riêng:

1. CloudTrail → Trails.
2. Stop logging.
3. Delete trail.
4. Ghi lại tên S3 bucket chứa log để xóa ở bước tiếp theo.

Không cần xóa Event History mặc định của tài khoản AWS.

## 9. S3

Xóa các bucket của dự án, ví dụ:

```
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

Với mỗi bucket:

5. Chọn Empty.
6. Xác nhận xóa toàn bộ object và version.
7. Quay lại danh sách bucket.
8. Chọn Delete.

Làm tương tự với bucket CloudTrail hoặc ALB logs nếu có.

## 10. Xóa phần mạng còn lại

Trong nexusoms-demo-vpc, lần lượt xóa:

9. Route trỏ đến NAT Gateway.
10. Custom route tables.
11. Bốn subnet:
    - public-a
    - public-b
    - private-a
    - private-b
12. Detach và xóa Internet Gateway.
13. Xóa custom security groups:
    - ALB security group
    - ECS security group
    - Endpoint security group
14. Cuối cùng xóa nexusoms-demo-vpc.

Nếu subnet hoặc VPC không xóa được, vào EC2 → Network Interfaces, lọc theo VPC để xác định ALB, ECS, NAT Gateway hoặc VPC Endpoint còn sót.

## 11. IAM và ACM

Sau khi ECS đã bị xóa, xóa:

```
nexusoms-demo-ecs-execution-role
nexusoms-demo-ecs-task-role
```

Nếu role còn inline policy, xóa policy trước rồi xóa role.

Kiểm tra ACM tại:

- ap-southeast-1 cho ALB
- us-east-1 cho CloudFront

Chỉ xóa certificate NexusOMS khi trường In use by đã trống.

Không xóa IAM user quản trị đang dùng cho các Workshop khác.

## 12. Kiểm tra cleanup

Kiểm tra các dịch vụ có chi phí cao trước:

- NAT Gateway: không còn
- Elastic IP: không còn IP không sử dụng
- VPC Interface Endpoint: không còn
- ALB: không còn
- ECS/Fargate task: 0
- WAF Web ACL: không còn
- CloudFront distribution: không còn

Cuối cùng vào:

```
Resource Groups & Tag Editor → Tag Editor
```

Chọn tất cả Region và tìm theo:

```
nexusoms
NexusOMS
Environment=demo
```

Cost Explorer có thể cập nhật chậm khoảng 24 giờ. Chi phí lịch sử vẫn xuất hiện, nhưng chi phí mới phải ngừng tăng sau khi các tài nguyên tính theo giờ đã bị xóa.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**ECS service name**

```text
nexusoms-demo-api-service
```

**Application Load Balancer value**

```text
nexusoms-demo-alb
```

**Xóa toàn bộ tài nguyên - giá trị 3**

```text
nexusoms-demo-api-tg
```

**ECS cluster name**

```text
nexusoms-demo-cluster
```

**Xóa toàn bộ tài nguyên - giá trị 5**

```text
nexusoms-demo-api
```

**Xóa toàn bộ tài nguyên - giá trị 6**

```text
nexusoms/demo/application
```

**Xóa toàn bộ tài nguyên - giá trị 7**

```text
/ecs/nexusoms-demo-api
```

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**VPC value**

```text
nexusoms-demo-vpc
```

**Xóa toàn bộ tài nguyên - giá trị 10**

```text
nexusoms-demo-deployer
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Disable Distribution

Trên AWS Console, thực hiện **Disable Distribution** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/1. Disable Distribution.png" title="Disable Distribution" >}}

### Bước 2 - Delete Distribution

Trên AWS Console, thực hiện **Delete Distribution** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/2. Delete Distribution.png" title="Delete Distribution" >}}

### Bước 3 - Delete CloudFront functions

Trên AWS Console, thực hiện **Delete CloudFront functions** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/3. Delete CloudFront functions.png" title="Delete CloudFront functions" >}}

### Bước 4 - Delete Stacks in CloudFormation

Trên AWS Console, thực hiện **Delete Stacks in CloudFormation** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/4. Delete Stacks in CloudFormation.png" title="Delete Stacks in CloudFormation" >}}

### Bước 5 - Delete cluster

Trên AWS Console, thực hiện **Delete cluster** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/5. Delete cluster.png" title="Delete cluster" >}}

### Bước 6 - Delete Target Group

Trên AWS Console, thực hiện **Delete Target Group** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/6. Delete Target Group.png" title="Delete Target Group" >}}

### Bước 7 - Delete Load Balancer

Trên AWS Console, thực hiện **Delete Load Balancer** như hình dưới đây.

**Giá trị dùng ở bước này**

**Application Load Balancer value**

```text
nexusoms-demo-alb
```

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/7. Delete Load Balancer.png" title="Delete Load Balancer" >}}

### Bước 8 - Delete NAT Gateway B

Trên AWS Console, thực hiện **Delete NAT Gateway B** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/8. Delete NAT Gateway B.png" title="Delete NAT Gateway B" >}}

### Bước 9 - Delete NAT Gateway A

Trên AWS Console, thực hiện **Delete NAT Gateway A** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/9. Delete NAT Gateway A.png" title="Delete NAT Gateway A" >}}

### Bước 10 - Release 2 Elastic IP addresses

Trên AWS Console, thực hiện **Release 2 Elastic IP addresses** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/10. Release 2 Elastic IP addresses.png" title="Release 2 Elastic IP addresses" >}}

### Bước 11 - Detach IGW from VPC

Trên AWS Console, thực hiện **Detach IGW from VPC** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/11. Detach IGW from VPC.png" title="Detach IGW from VPC" >}}

### Bước 12 - Delete VPC

Trên AWS Console, thực hiện **Delete VPC** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/12. Delete VPC.png" title="Delete VPC" >}}

### Bước 13 - Choose Nexusoms-demo-api task

Trên AWS Console, thực hiện **Choose Nexusoms-demo-api task** như hình dưới đây.

**Giá trị dùng ở bước này**

**ECS service name**

```text
nexusoms-demo-api-service
```

**Xóa toàn bộ tài nguyên - giá trị 3**

```text
nexusoms-demo-api-tg
```

**ECS cluster name**

```text
nexusoms-demo-cluster
```

**Xóa toàn bộ tài nguyên - giá trị 5**

```text
nexusoms-demo-api
```

**Xóa toàn bộ tài nguyên - giá trị 6**

```text
nexusoms/demo/application
```

**Xóa toàn bộ tài nguyên - giá trị 7**

```text
/ecs/nexusoms-demo-api
```

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**VPC value**

```text
nexusoms-demo-vpc
```

**Xóa toàn bộ tài nguyên - giá trị 10**

```text
nexusoms-demo-deployer
```

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/13. Choose Nexusoms-demo-api task.png" title="Choose Nexusoms-demo-api task" >}}

### Bước 14 - Deregister the Task revision

Trên AWS Console, thực hiện **Deregister the Task revision** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/14. Deregister the Task revision.png" title="Deregister the Task revision" >}}

### Bước 15 - Delete Repositories

Trên AWS Console, thực hiện **Delete Repositories** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/15. Delete Repositories.png" title="Delete Repositories" >}}

### Bước 16 - Open detail Secrets Manager

Trên AWS Console, thực hiện **Open detail Secrets Manager** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/16. Open detail Secrets Manager.png" title="Open detail Secrets Manager" >}}

### Bước 17 - Select waiting period + schedule deletion

Trên AWS Console, thực hiện **Select waiting period + schedule deletion** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/17. Select waiting period + schedule deletion.png" title="Select waiting period + schedule deletion" >}}

### Bước 18 - Delete log groups in CloudWatch

Trên AWS Console, thực hiện **Delete log groups in CloudWatch** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/18. Delete log groups in CloudWatch.png" title="Delete log groups in CloudWatch" >}}

### Bước 19 - Empty S3 bucket

Trên AWS Console, thực hiện **Empty S3 bucket** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/19. Empty S3 bucket.png" title="Empty S3 bucket" >}}

### Bước 20 - Delete S3 bucket

Trên AWS Console, thực hiện **Delete S3 bucket** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/20. Delete S3 bucket.png" title="Delete S3 bucket" >}}

### Bước 21 - Delete 2 role that create

Trên AWS Console, thực hiện **Delete 2 role that create** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/21. Delete 2 role that create.png" title="Delete 2 role that create" >}}
