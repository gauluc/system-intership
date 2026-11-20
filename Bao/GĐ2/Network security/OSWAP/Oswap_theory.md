# 1. OSWAP

**OSWAP - Open Worldwide Application Security Project** là một tổ chức cộng đồng phi lợi nhuận hoạt động nhằm cải thiện bảo mật phần mềm

OWASP cung cấp:

- Tài liệu về các lỗ hổng bảo mật
- Công cụ kiểm thử bảo mật
- Hướng dẫn Secure Coding
- Framework đánh giá bảo mật
- Các tiêu chuẩn và dự án mã nguồn mở

Một số dự án quan trọng:

|Dự án	|Chức năng|
| :--- | :--- |
|OWASP Top 10	|Danh sách rủi ro bảo mật ứng dụng|
|OWASP ASVS	|Tiêu chuẩn xác minh bảo mật ứng dụng|
|OWASP API Security Top 10	|Rủi ro bảo mật API|
|OWASP ZAP	|Công cụ kiểm thử bảo mật Web|
|OWASP Dependency-Check	|Kiểm tra thư viện có lỗ hổng|
|OWASP Cheat Sheet Series	|Hướng dẫn triển khai bảo mật|

# 2. OSWAP top 10

OWASP Top 10 là danh sách tổng hợp những nhóm rủi ro bảo mật ứng dụng Web quan trọng, dựa trên dữ liệu và phân tích của cộng đồng bảo mật

Mục đích:

1. Giúp lập trình viên nhận biết các loại lỗ hổng phổ biến
2. Giúp đội ngũ Security đánh giá rủi ro
3. Giúp xây dựng Secure Software Development Lifecycle (SSDLC)
4. Hỗ trợ kiểm thử bảo mật trước khi triển khai ứng dụng
5. Giúp doanh nghiệp ưu tiên khắc phục các vấn đề bảo mật

### OSWAP top 10 năm 2025 ( phiên bản mới nhất )

|Mã số |Nhóm rủi ro|
| :--- | :--- |
|A01 |Broken Access Control - Kiểm soát truy cập bị lỗi|
|A02 |Security Misconfiguration - Cấu hình bảo mật sai|
|A03 |Software Supply Chain Failures - Lỗi trong chuỗi cung ứng phần mềm|
|A04 |Cryptographic Failures - Lỗi mã hóa|
|A05 |Injection - Tấn công chèn mã|
|A06 |Insecure Design - Thiết kế không an toàn|
|A07 |Authentication Failures - Lỗi xác thực|
|A08 |Software or Data Integrity Failures - Lỗi toàn vẹn phần mềm hoặc dữ liệu|
|A09 |Security Logging & Alerting Failures - Lỗi ghi log và cảnh báo bảo mật|
|A10 |Mishandling of Exceptional Conditions - Xử lý điều kiện bất thường không đúng|

### 2.1. A01 – Broken Access Control

Broken Access Control là tình trạng ứng dụng không kiểm soát đúng quyền truy cập của người dùng đối với tài nguyên hoặc chức năng

Ví dụ, một người dùng bình thường có thể truy cập chức năng chỉ dành cho Admin

Các dạng tấn công thường gặp:

- IDOR (Insecure Direct Object Reference)
- Privilege Escalation
- Path Traversal
- Bypass Authorization
- Forced Browsing
- SSRF được xếp trong nhóm này ở phiên bản 2025

**Ví dụ thực tế:**

Một API lấy thông tin Firewall:

```
GET /api/firewalls/1001
Authorization: Bearer USER_A_TOKEN
```

API trả về Firewall có ID 1001

Nếu người dùng A thay đổi thành:

```
GET /api/firewalls/1002
Authorization: Bearer USER_A_TOKEN
```

và hệ thống trả về Firewall của người dùng B thì ứng dụng có lỗi phân quyền

Vấn đề nằm ở hệ thống chỉ kiểm tra người dùng đã đăng nhập mà không kiểm tra quyền sở hữu tài nguyên

Nguyên nhân:

- Chỉ kiểm tra Authentication, không kiểm tra Authorization
- Không kiểm tra quyền ở Backend
- Tin tưởng dữ liệu ID do Client gửi lên
- Thiếu kiểm soát quyền giữa các Tenant

Biện pháp phòng chống:

1. Kiểm tra quyền tại Server-side
2. Sử dụng RBAC hoặc ABAC
3. Kiểm tra quyền sở hữu từng tài nguyên
4. Áp dụng nguyên tắc Least Privilege
5. Kiểm tra quyền ở mọi API Endpoint

### 2.2. A02 – Security Misconfiguration

Lỗi cấu hình bảo mật xảy ra khi hệ thống, ứng dụng, Server hoặc dịch vụ được cấu hình không an toàn

Ví dụ:

- Để mặc định tài khoản Admin
- Mở các Port không cần thiết
- Bật Debug Mode trên Production
- Cho phép truy cập Swagger không giới hạn
- Cấu hình CORS quá rộng
- Không giới hạn HTTP Request Size
- Để lộ thông tin lỗi nội bộ
- Không cấu hình Security Header

**Ví dụ:**

Một ứng dụng ASP.NET Core chạy Production nhưng vẫn bật:

```
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug"
    }
  }
}
```

Hoặc sử dụng:

```
app.UseSwagger();
app.UseSwaggerUI();
```

mà không có cơ chế kiểm soát truy cập phù hợp

Kẻ tấn công có thể thu thập thông tin API, Endpoint, cấu trúc dữ liệu và các chức năng nội bộ

Biện pháp phòng chống:

- Xây dựng Baseline Configuration
- Tắt Debug Mode trên Production
- Xóa tài khoản mặc định
- Chỉ mở Port cần thiết
- Bật HTTPS
- Cấu hình CORS theo Allowlist
- Kiểm tra cấu hình định kỳ
- Sử dụng CIS Benchmarks để Hardening hệ thống

### 2.3. A03 – Software Supply Chain Failures

Lỗi chuỗi cung ứng phần mềm xảy ra khi các thành phần tham gia vào quá trình phát triển, xây dựng, phân phối hoặc cập nhật phần mềm bị xâm phạm hoặc không được kiểm soát an toàn

Bao gồm:

- Thư viện bên thứ ba có lỗ hổng
- NuGet Package độc hại
- NPM Package giả mạo
- Dependency bị chiếm quyền
- Build Pipeline bị xâm nhập
- CI/CD sử dụng Secret không an toàn
- Container Image chứa malware
- Software Update không đáng tin cậy

**Ví dụ với .NET:**

Ứng dụng sử dụng:

```
<PackageReference
    Include="Example.Library"
    Version="1.0.0" />
```

Nếu Package có lỗ hổng nghiêm trọng, ứng dụng có thể bị ảnh hưởng ngay cả khi code do đội ngũ nội bộ viết không có lỗi

Biện pháp phòng chống:

1. Kiểm tra Dependency
2. Cập nhật Package thường xuyên
3. Sử dụng Software Composition Analysis (SCA)
4. Kiểm tra nguồn gốc Package
5. Sử dụng SBOM (Software Bill of Materials)
6. Ký và xác minh Artifact
7. Bảo vệ CI/CD Pipeline

### 2.4. A04 – Cryptographic Failures

Lỗi mật mã xảy ra khi ứng dụng không bảo vệ dữ liệu bằng cơ chế mã hóa phù hợp

Các lỗi thường gặp:

- Truyền dữ liệu nhạy cảm qua HTTP
- Sử dụng thuật toán mã hóa yếu
- Lưu Password dạng Plain Text
- Sử dụng MD5 hoặc SHA-1 cho mục đích không còn an toàn
- Hardcode Encryption Key trong Source Code
- Không quản lý vòng đời Encryption Key
- Không xác minh TLS Certificate

Ví dụ nguy hiểm: `string password = user.Password;`

Nếu lưu trực tiếp password vào Database, khi Database bị lộ, toàn bộ mật khẩu người dùng có thể bị khai thác

Cách khắc phục:

- Sử dụng HTTPS/TLS
- Dùng Argon2id, bcrypt hoặc PBKDF2 để Hash Password
- Dùng AES-GCM khi cần mã hóa dữ liệu đối xứng
- Quản lý Key bằng Secret Manager hoặc KMS
- Không lưu Secret trong Git
- Phân loại dữ liệu và xác định dữ liệu cần mã hóa

### 2.5. A05 – Injection

Injection xảy ra khi ứng dụng đưa dữ liệu không đáng tin cậy vào một câu lệnh hoặc trình thông dịch mà không xử lý đúng cách

Các loại phổ biến:

|Loại	|Mục tiêu|
| :--- | :--- |
|SQL Injection	|Database SQL|
|NoSQL Injection	|NoSQL Database|
|OS Command Injection	|Hệ điều hành|
|LDAP Injection	|LDAP|
|XPath Injection	|XML/XPath|
|Server-Side Template Injection	|Template Engine|

**Ví dụ SQL Injection:**

Một ứng dụng xây dựng SQL bằng cách nối chuỗi:

```
string sql =
    "SELECT * FROM Users WHERE Username = '" +
    username + "'";
```

Nếu dữ liệu đầu vào không được xử lý đúng, kẻ tấn công có thể thay đổi ý nghĩa câu truy vấn

Biện pháp phòng chống:

Sử dụng Parameterized Query:

```
var user = await db.Users
    .FirstOrDefaultAsync(x =>
        x.Username == username);
```

Ngoài ra:

- Validate dữ liệu đầu vào
- Không nối chuỗi để xây dựng SQL
- Hạn chế quyền của tài khoản Database
- Không thực thi dữ liệu người dùng như mã lệnh
- Sử dụng ORM đúng cách
- Kiểm thử bằng SAST và DAST

6. A06 – Insecure Design

Insecure Design là lỗi xuất phát từ việc thiết kế ứng dụng không tính đến các tình huống tấn công hoặc các yêu cầu bảo mật ngay từ đầu

Khác với Security Misconfiguration:

- Security Misconfiguration: Thiết kế có thể đúng nhưng cấu hình triển khai sai
- Insecure Design: Bản thân thiết kế đã thiếu cơ chế bảo vệ

**Ví dụ:**

Một hệ thống Cloud Firewall cho phép khách hàng tạo Firewall không giới hạn

Không có:

- Rate Limit
- Quota
- Giới hạn số lượng Rule
- Kiểm soát tài nguyên
- Kiểm tra quyền quản trị

Kẻ tấn công có thể lạm dụng chức năng hợp lệ để gây quá tải hệ thống

Biện pháp phòng chống:

1. Threat Modeling
2. Security Requirement
3. Secure Architecture
4. Least Privilege
5. Defense in Depth
6. Rate Limiting
7. Resource Quota
8. Abuse Case Testing

### 2.7. A07 – Authentication Failures

Lỗi xác thực xảy ra khi ứng dụng không xác minh đúng danh tính người dùng hoặc quản lý Session, Token và thông tin đăng nhập không an toàn

Các lỗi thường gặp:

- Brute Force
- Credential Stuffing
- Password yếu
- Không có MFA
- JWT không được xác minh đúng
- Token hết hạn nhưng vẫn được chấp nhận
- Refresh Token không được quản lý
- Session không được thu hồi sau khi Logout

**Ví dụ JWT:**

Một API chỉ giải mã JWT mà không xác minh chữ ký

Kẻ tấn công có thể sửa các trường:

```
{
  "sub": "user01",
  "role": "Admin"
}
```

Nếu Server tin tưởng payload mà không xác minh chữ ký và các claim cần thiết, kẻ tấn công có thể giả mạo quyền

Biện pháp phòng chống:

- Sử dụng OAuth 2.0 / OpenID Connect phù hợp
- Xác minh chữ ký JWT
- Kiểm tra iss, aud, exp, nbf
- Sử dụng MFA
- Giới hạn số lần đăng nhập
- Có cơ chế khóa hoặc trì hoãn đăng nhập
- Quản lý Refresh Token
- Không lưu Password dạng Plain Text

### 2.8. A08 – Software or Data Integrity Failures

Lỗi toàn vẹn phần mềm hoặc dữ liệu xảy ra khi hệ thống không xác minh được dữ liệu, mã nguồn, phần mềm hoặc Artifact có bị thay đổi trái phép hay không

Ví dụ:

- Không xác minh chữ ký phần mềm
- Cập nhật ứng dụng từ nguồn không đáng tin cậy
- CI/CD cho phép thay đổi Artifact trái phép
- Deserialization dữ liệu không đáng tin cậy
- Tin tưởng dữ liệu được gửi từ hệ thống bên ngoài mà không xác minh

Biện pháp phòng chống:

- Digital Signature
- Hash Verification
- Artifact Signing
- Secure CI/CD
- Kiểm tra tính toàn vẹn dữ liệu
- Hạn chế quyền ghi vào Repository
- Xác minh nguồn gốc và tính toàn vẹn của bản cập nhật

### 2.9. A09 – Security Logging & Alerting Failures

Hệ thống không ghi nhận đầy đủ các sự kiện bảo mật hoặc không tạo cảnh báo khi phát hiện hoạt động bất thường

Ví dụ:
- Không ghi nhận đăng nhập thất bại
- Không ghi nhận thay đổi quyền Admin
- Không ghi nhận tạo hoặc xóa Firewall
- Không cảnh báo khi có nhiều lần đăng nhập sai
- Log bị xóa hoặc chỉnh sửa
- Log không được tập trung về SIEM

Biện pháp phòng chống:

Ghi nhận các sự kiện:

|Sự kiện	|Thông tin cần ghi|
| :--- | :--- |
|Login	|User, IP, thời gian, kết quả|
|Logout	|User, thời gian|
|Login failed	|Username, IP, lý do phù hợp|
|Privilege change	|Người thay đổi, quyền cũ/mới|
|Firewall create	|User, Customer, Firewall ID|
|Firewall delete	|Người thực hiện, đối tượng|
|API access	|Endpoint, status code, thời gian|

Sử dụng các giải pháp:

- SIEM
- Wazuh
- Splunk
- Microsoft Sentinel
- ELK Stack
- Grafana Loki

Không nên ghi Password, Access Token hoặc Secret trực tiếp vào Log

### 2.10. A10 – Mishandling of Exceptional Conditions

Ứng dụng xử lý không đúng các điều kiện bất thường, lỗi hệ thống hoặc trạng thái ngoài dự kiến

Các vấn đề thường gặp:

- Không xử lý Exception
- Fail-open khi xảy ra lỗi
- Race Condition
- Không kiểm tra trạng thái dữ liệu
- Xử lý Timeout không an toàn
- Không xử lý đúng giao dịch thất bại
- Logic nghiệp vụ bị sai khi xảy ra lỗi

**Ví dụ:**

Một hệ thống kiểm tra quyền truy cập Firewall:

```
try
{
    return await CheckPermission(userId);
}
catch
{
    return true;
}
```

Khi dịch vụ kiểm tra quyền bị lỗi, ứng dụng lại cho phép truy cập

Đây là lỗi Fail-open rất nguy hiểm

Cách xử lý tốt hơn:

```
try
{
    return await CheckPermission(userId);
}
catch (Exception ex)
{
    logger.LogError(ex,
        "Permission validation failed");

    return false;
}
```

Đối với các chức năng nhạy cảm, khi không thể xác minh quyền thì phải từ chối truy cập

Ngoài ra cần:

- Xử lý Timeout
- Retry có giới hạn
- Circuit Breaker
- Transaction và Rollback
- Idempotency
- Kiểm tra trạng thái trước khi thực hiện thao tác
