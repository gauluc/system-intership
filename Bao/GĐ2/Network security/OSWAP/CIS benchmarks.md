# 1. CIS

CIS - Center for Internet Security là tổ chức phát triển các hướng dẫn và giải pháp nhằm cải thiện khả năng bảo vệ hệ thống thông tin

CIS cung cấp hai tài liệu quan trọng:

1. CIS Controls
2. CIS Benchmarks

Hai khái niệm này liên quan với nhau nhưng không giống nhau

# 2. CIS Benchmarks

CIS Benchmarks là bộ hướng dẫn cấu hình bảo mật được xây dựng dựa trên sự đồng thuận của các chuyên gia an ninh mạng

Mục tiêu là đưa ra những cấu hình cụ thể giúp hệ thống chống lại các nguy cơ tấn công

Ví dụ: Bạn cài đặt Ubuntu Server 24.04

Sau khi cài đặt, hệ thống có thể tồn tại các vấn đề:

- SSH cho phép đăng nhập bằng Root
- Firewall chưa được bật
- Cho phép nhiều dịch vụ không cần thiết
- Password Policy chưa đủ mạnh
- Log chưa được cấu hình đầy đủ
- Quyền truy cập File và Directory quá rộng

CIS Benchmark cung cấp các khuyến nghị cụ thể để kiểm tra và điều chỉnh các cấu hình đó

Theo CIS, bộ Benchmarks hiện có hơn 100 tài liệu, bao phủ hơn 25 nhóm sản phẩm công nghệ

# 3. Áp dụng 

### 3.1. Operating Systems

- Windows 10/11
- Windows Server
- Ubuntu
- Debian
- Red Hat Enterprise Linux
- Rocky Linux
- SUSE Linux

### 3.2. Network Devices

- Cisco Router/Switch
- Fortinet
- Palo Alto
- Juniper
- Check Point
- pfSense

### 3.3. Cloud Platforms

- AWS
- Microsoft Azure
- Google Cloud
- Oracle Cloud
- Microsoft 365

### 3.4. Container và DevOps

- Docker
- Kubernetes
- Software Supply Chain Security
- Container Images

### 3.5. Database và Server Software

- PostgreSQL
- Microsoft SQL Server
- MySQL
- MongoDB
- NGINX
- Apache
- IIS

# 4. Cấu trúc CIS Benchmark

Một CIS Benchmark thường bao gồm nhiều khuyến nghị (Recommendations), mỗi khuyến nghị hướng dẫn một yêu cầu cấu hình bảo mật

Một Recommendation thường có các nội dung:

1. Title: Tên yêu cầu
2. Assessment Status: Kiểm tra tự động hay thủ công
3. Profile Applicability: Áp dụng cho Profile nào
4. Description: Mô tả yêu cầu
5. Rationale: Giải thích tại sao cần cấu hình
6. Impact: Ảnh hưởng khi triển khai
7. Audit: Hướng dẫn kiểm tra
8. Remediation: Hướng dẫn khắc phục
9. References: Tài liệu tham khảo

# 5. CIS Benchmark Level 1 và Level 2

Đây là nội dung cần nắm rõ khi triển khai CIS

CIS chia các khuyến nghị thành các mức Profile, thường gặp nhất là Level 1 và Level 2

```
CIS Level 1

L1
Mục tiêu: Cải thiện bảo mật mà không gây ảnh hưởng lớn đến hoạt động thông thường

Ví dụ:

- Tắt dịch vụ không cần thiết
- Thiết lập Password Policy
- Cấu hình Firewall
- Hạn chế SSH Root Login
- Bật Logging
```

```
CIS Level 2

L2
Mục tiêu: Cung cấp mức bảo vệ nghiêm ngặt hơn cho các hệ thống cần bảo mật cao

Ví dụ:

- Hạn chế thêm các giao thức
- Kiểm soát truy cập nghiêm ngặt hơn
- Tăng cường Auditing
- Áp dụng các chính sách bảo mật chặt chẽ
- Giới hạn các dịch vụ và chức năng hệ thống
```

**Khác biệt chính:**

|Tiêu chí	|Level 1	|Level 2|
| :--- | :--- | :--- |
|Mức bảo mật	|Cơ bản	|Nâng cao|
|Tính tương thích	|Cao hơn	|Có thể thấp hơn|
|Ảnh hưởng hoạt động	|Thường thấp	|Có thể đáng kể|
|Đối tượng	|Hầu hết hệ thống	|Hệ thống nhạy cảm|
|Triển khai	|Nên bắt đầu	|Cần đánh giá kỹ|

Không phải mọi Recommendation đều có cả hai Level. Việc áp dụng cần dựa vào đúng Benchmark, phiên bản và Profile của sản phẩm

# 6. CIS Controls và CIS Benchmarks khác nhau như thế nào?

|Tiêu chí	|CIS Controls	|CIS Benchmarks|
| :--- | :--- | :--- |
|Mục tiêu	|Xác định các biện pháp bảo vệ an ninh mạng	|Xác định cấu hình an toàn|
|Phạm vi	|Toàn tổ chức	|Từng sản phẩm/hệ thống|
|Nội dung	|Những gì cần bảo vệ	|Cấu hình như thế nào|
|Ví dụ	|Quản lý tài khoản, tài sản	|Cấu hình SSH, Firewall|
|Đối tượng	|Security Manager, IT Manager	|System Admin, Security Engineer|

# 7. Công cụ kiểm tra CIS Benchmark

Có nhiều công cụ hỗ trợ đánh giá cấu hình hệ thống theo CIS Benchmark

### 7.1. CIS-CAT

Công cụ đánh giá cấu hình hệ thống so với các Recommendation trong CIS Benchmark

Chức năng:

- Kiểm tra cấu hình hệ thống
- Đánh giá mức độ tuân thủ
- Phát hiện cấu hình sai
- Xuất báo cáo
- Hỗ trợ đánh giá Level 1 và Level 2

CIS-CAT Pro là sản phẩm thương mại thuộc CIS SecureSuite; khả năng sử dụng phụ thuộc giấy phép

### 7.2. OpenSCAP

Framework mã nguồn mở hỗ trợ đánh giá cấu hình và tuân thủ bảo mật

Chức năng:

- Kiểm tra cấu hình Linux
- Đánh giá Security Compliance
- Sử dụng SCAP Content
- Xuất báo cáo
- Hỗ trợ tự động hóa kiểm tra

### 7.3 Wazuh

Nền tảng giám sát an ninh hỗ trợ kiểm tra cấu hình và phát hiện sai lệch bảo mật.
Wazuh chính thức

Chức năng:

- Security Configuration Assessment (SCA)
- File Integrity Monitoring
- Vulnerability Detection
- Log Analysis
- Security Alerting

Wazuh có các policy SCA được xây dựng dựa trên CIS, nhưng không nên mặc định rằng mọi policy đều tương ứng đầy đủ với phiên bản CIS Benchmark mới nhất

# 8. Quy trình triển khai CIS Benchmark

Một quy trình triển khai thực tế có thể gồm 7 bước:

1. Inventory – Kiểm kê hệ thống
   
Xác định OS, Version, Server Role, Network Device và các dịch vụ đang chạy

2. Select Benchmark – Chọn tiêu chuẩn

Chọn đúng sản phẩm, phiên bản và Profile Level 1 hoặc Level 2

3. Baseline Assessment – Đánh giá hiện trạng

Kiểm tra cấu hình hiện tại, thu thập kết quả Pass/Fail/Manual

4. Risk Assessment – Phân tích rủi ro

Xác định Recommendation nào cần ưu tiên và có ảnh hưởng đến nghiệp vụ hay không

5. Remediation – Khắc phục

Thay đổi cấu hình, áp dụng Policy và loại bỏ các cấu hình không an toàn

6. Validation – Kiểm tra lại

Quét lại hệ thống để xác nhận các cấu hình đã đạt yêu cầu

7. Continuous Monitoring – Giám sát liên tục

Định kỳ đánh giá lại và phát hiện cấu hình bị thay đổi

