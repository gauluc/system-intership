# 1. Bot protection

**Bot Protection** là nhóm chức năng bảo mật giúp phát hiện, phân loại, giám sát và kiểm soát lưu lượng truy cập được tạo ra bởi các chương trình tự động (Bot)

Bot không phải lúc nào cũng độc hại

Có hai nhóm chính:

1. Good Bot: Bot hợp pháp, phục vụ tìm kiếm, giám sát, thu thập dữ liệu được cho phép

2. Bad Bot: Bot độc hại, tự động khai thác, spam, dò mật khẩu hoặc lạm dụng dịch vụ

**Ví dụ:**

Good Bot:

- Googlebot thu thập dữ liệu website
- Bot giám sát tình trạng hoạt động của hệ thống
- Bot kiểm tra SEO được cấp phép

Bad Bot:

- Bot dò mật khẩu tài khoản
- Bot tạo hàng nghìn tài khoản giả
- Bot thu thập dữ liệu trái phép
- Bot tự động mua hàng để đầu cơ
- Bot gửi request liên tục làm quá tải Server

## 1.1. Các chức năng chính của Bot Protection

### Bot Detection

Phát hiện Bot thông qua nhiều phương pháp:

1. User-Agent Detection

Kiểm tra HTTP User-Agent

Ví dụ: `User-Agent: Python-Requests/2.31`

Một số Bot sử dụng User-Agent bất thường hoặc giả mạo trình duyệt

Tuy nhiên, chỉ kiểm tra User-Agent là chưa đủ vì Bot có thể giả lập trình duyệt thật

2. IP Reputation

Đánh giá độ tin cậy của IP:

- IP từng thực hiện tấn công
- IP thuộc Botnet
- IP có hành vi gửi request bất thường
- IP thuộc nguồn Proxy hoặc Datacenter đáng ngờ

3. Behavioral Analysis

Phân tích hành vi truy cập

Ví dụ:

| Người dùng bình thường | Bot |
| :--- | :--- |
| Thời gian giữa các request thay đổi | Gửi request theo chu kỳ cố định |
| Di chuyển chuột | Không có tương tác phù hợp |
| Có hành vi điều hướng tự nhiên | Truy cập endpoint theo mẫu lặp |
| Có quá trình nhập dữ liệu | Tự động gửi hàng loạt dữ liệu |

Không phải mọi Bot đều không có tương tác chuột hoặc đều gửi request theo chu kỳ cố định. Bot hiện đại có thể giả lập trình duyệt và hành vi người dùng

4. Browser Fingerprinting

Thu thập và phân tích một số đặc điểm trình duyệt:

- Browser Type
- Browser Version
- JavaScript Support
- TLS Fingerprint
- HTTP Header Characteristics
- Cookie Behavior

Từ đó đánh giá khả năng request được gửi từ trình duyệt thật hay công cụ tự động

### CAPTCHA Challenge

CAPTCHA là cơ chế yêu cầu Client thực hiện một thử thách để xác minh hành vi truy cập

Ví dụ:

- Chọn hình ảnh theo yêu cầu
- Xác nhận checkbox
- Giải CAPTCHA
- Xác minh bằng JavaScript Challenge

### Rate Limiting và Request Throttling

Giới hạn số lượng request theo:

- IP Address
- User ID
- Session ID
- API Key
- Endpoint
- Device Fingerprint

### Credential Stuffing Protection

Credential Stuffing là hình thức sử dụng thông tin đăng nhập bị rò rỉ từ hệ thống khác để thử đăng nhập vào hệ thống mục tiêu

Bot Protection có thể:

- Phát hiện đăng nhập hàng loạt
- Phân tích tỷ lệ đăng nhập thất bại
- Phát hiện nhiều tài khoản bị thử từ cùng một nguồn
- Phát hiện một tài khoản bị thử từ nhiều nguồn
- Yêu cầu CAPTCHA hoặc MFA bổ sung
- Tạm khóa hoặc giới hạn truy cập

### Scraping Protection

Scraping là hoạt động sử dụng Bot để thu thập dữ liệu tự động

Nếu Bot gửi hàng triệu request để lấy toàn bộ dữ liệu sản phẩm, hệ thống có thể bị lạm dụng

Bot Protection có thể:

- Phát hiện truy cập bất thường
- Giới hạn tốc độ thu thập
- Chặn IP hoặc Fingerprint độc hại
- Yêu cầu CAPTCHA
- Giới hạn truy cập dữ liệu nhạy cảm

### Bot Classification

Phân loại Bot thành các nhóm:

| Phân loại | Hành động |
| :--- | :--- |
| Verified Good Bot | Allow |
| Unknown Bot | Monitor hoặc Challenge |
| Suspicious Bot | Challenge hoặc Rate Limit |
| Malicious Bot | Block |

## 1.2. Ưu và nhược điểm

Ưu điểm:

- Ngăn chặn Brute Force
- Giảm Credential Stuffing
- Hạn chế Scraping
- Giảm lạm dụng tài nguyên
- Bảo vệ tài khoản người dùng
- Hạn chế Bot tạo tài khoản giả

Nhược điểm:

- Bot hiện đại có thể giả lập người dùng thật
- Có thể chặn nhầm người dùng hợp pháp
- CAPTCHA gây ảnh hưởng trải nghiệm
- Cần cập nhật mô hình phát hiện liên tục

# 2. API protection

**API Protection** là tập hợp các cơ chế bảo mật giúp bảo vệ Application Programming Interface (API) trước các cuộc tấn công, truy cập trái phép và hành vi lạm dụng tài nguyên

Nếu API không được bảo vệ, kẻ tấn công có thể:
- Truy cập dữ liệu người dùng khác
- Giả mạo danh tính
- Gọi API quản trị trái phép
- Lạm dụng API để tiêu tốn tài nguyên
- Khai thác API nội bộ
- Lấy thông tin nhạy cảm

Điểm khác biệt: WAF chủ yếu tập trung vào việc phân tích và chặn các request Web độc hại. API Protection tập trung sâu hơn vào cấu trúc, danh tính, quyền truy cập, dữ liệu và logic nghiệp vụ của API

## 2.1. Kiến trúc API Protection

```
                 INTERNET
                     |
                     v
                API CLIENT
                     |
                     v
             +---------------+
             | API GATEWAY   |
             +---------------+
                     |
           +---------+---------+
           |                   |
           v                   v
     Authentication      Rate Limiting
           |                   |
           +---------+---------+
                     |
                     v
             +---------------+
             | API SECURITY  |
             +---------------+
                     |
          +----------+----------+
          |          |           |
          v          v           v
       WAF Rule   Schema      Access
                  Validation  Control
          |          |           |
          +----------+----------+
                     |
                     v
               BACKEND API
                     |
          +----------+----------+
          |          |           |
          v          v           v
       Users API  Orders API  Firewall API
```

## 2.2. Các chức năng chính của API Protection

### API Discovery

API Discovery là chức năng tự động phát hiện các API đang tồn tại trong hệ thống

Ví dụ:

```
/api/users
/api/users/{id}
/api/auth/login
/api/firewall/create
/api/firewall/delete
/api/subscriptions
```

Mục đích:

- Phát hiện API đang hoạt động
- Phát hiện API chưa được bảo vệ
- Phát hiện API cũ (Deprecated API)
- Phát hiện Shadow API
- Xây dựng danh mục API Inventory

*Shadow API* là API đang hoạt động nhưng không được quản lý hoặc không nằm trong danh mục API chính thức

*Zombie API* là API đã cũ hoặc đáng lẽ phải ngừng hoạt động nhưng vẫn còn được truy cập

### Authentication Protection

Authentication là quá trình xác thực danh tính Client hoặc User

Một số cơ chế phổ biến:

| Cơ chế | Mục đích |
| :--- | :--- |
| API | Key	Xác định ứng dụng hoặc Client |
| JWT | Xác thực bằng Token |
| OAuth 2.0 | Cấp quyền truy cập có kiểm soát |
| OpenID Connect | Xác thực danh tính dựa trên OAuth 2.0 |
| mTLS | Xác thực hai chiều bằng chứng chỉ |

### Authorization Protection

Authorization là quá trình kiểm tra quyền truy cập

Ví dụ:

User A: `GET /api/users/1001`

User B: `GET /api/users/1002`

Nếu User A cố tình thay ID 1001 thành 1002 và đọc được dữ liệu của User B, hệ thống có thể mắc lỗi BOLA (Broken Object Level Authorization), còn thường được gọi là IDOR trong một số trường hợp

API Protection cần hỗ trợ kiểm soát:

- Role-Based Access Control (RBAC)
- Attribute-Based Access Control (ABAC)
- Object-Level Authorization
- Function-Level Authorization
- Tenant-Level Isolation

### Schema Validation

Schema Validation kiểm tra cấu trúc request có đúng với định nghĩa API hay không

Schema Validation có thể kiểm tra:

- JSON Schema
- XML Schema
- Required Fields
- Data Types
- Parameter Constraints
- Request Size
- Content-Type
- Allowed HTTP Methods

### API Rate Limiting và Quota Management

Hai chức năng này thường đi cùng nhau nhưng có ý nghĩa khác nhau

Rate Limiting: Giới hạn tốc độ request

Ví dụ: 100 requests / minute

Quota Management: Giới hạn tổng lượng sử dụng trong một khoảng thời gian

Ví dụ: 10,000 requests / month

Có thể giới hạn theo:

- API Key
- User
- Tenant
- Subscription
- Client IP
- API Endpoint

### API Threat Detection

Phát hiện những hành vi tấn công đặc thù đối với API

Các nguy cơ quan trọng theo OWASP API Security Top 10 – 2023:

| Mã | Tên lỗ hổng | Ý nghĩa |
| :--- | :--- | :--- |
| API1 | BOLA | Truy cập đối tượng không có quyền |
| API2 | Broken Authentication | Xác thực yếu hoặc sai |
| API3 | Broken Object Property Level Authorization | Truy cập hoặc sửa thuộc tính trái phép |
| API4 | Unrestricted Resource Consumption | Không giới hạn tài nguyên |
| API5 | Broken Function Level Authorization | Truy cập chức năng không có quyền |
| API6 | Unrestricted Access to Sensitive Business Flows | Lạm dụng quy trình nghiệp vụ |
| API7 | SSRF | Ép Server gửi request đến tài nguyên khác |
| API8 | Security Misconfiguration | Cấu hình bảo mật sai |
| API9 | Improper Inventory Management | Quản lý API không đầy đủ |
| API10 | Unsafe Consumption of APIs | Tin tưởng dữ liệu API bên ngoài không an toàn |

=> Đây là danh sách rất quan trọng khi thiết kế hệ thống bảo vệ API

### API Encryption

Bảo vệ dữ liệu trong quá trình truyền tải:

- HTTPS/TLS
- Mutual TLS
- Certificate Validation
- Token Protection

### API Logging và Monitoring

Ghi nhận hoạt động API để phục vụ giám sát và điều tra

Thông tin có thể ghi nhận:

- Timestamp
- Source IP
- HTTP Method
- API Endpoint
- HTTP Status Code
- User ID
- API Key ID
- Response Time
- Request Size
- Threat Category
- Action

## 2.3. API Protection bảo vệ những gì?

| Đối tượng	| Cơ chế|
| :--- | :--- |
| API Endpoint	| API Discovery, Access Control|
| API User	| Authentication|
| API Permission	| Authorization|
| Request Body	| Schema Validation|
| API Resource	| Rate Limiting, Quota|
| API Traffic	| WAF, Threat Detection|
| Sensitive Data	| Encryption, Data Filtering|
| API Activity	| Logging, Monitoring |

## 2.4. So sánh WAF và API Protection 

|Tiêu chí	|WAF	|API Protection|
| :--- | :--- | :--- |
|Mục tiêu	|Bảo vệ ứng dụng Web	|Bảo vệ API|
|SQL Injection	|Có	|Có thể có|
|XSS	|Có	|Có thể có|
|JWT Validation	|Tùy sản phẩm	|Thường có|
|API Discovery	|Không phải chức năng cốt lõi	|Có|
|Schema Validation	|Có thể hỗ trợ	|Là chức năng quan trọng|
|Object Authorization	|Hạn chế	|Phải được kiểm soát ở ứng dụng|
|API Rate Limiting	|Có	|Có|
|API Inventory	|Không phải chức năng cốt lõi	|Có|

Kết luận: WAF và API Protection bổ sung cho nhau. Nhiều sản phẩm hiện đại tích hợp cả hai trong cùng một nền tảng

