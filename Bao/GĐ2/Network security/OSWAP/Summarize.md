# 1. So sánh OWASP TOP 10 và CIS BENCHMARK trong kiến trúc bảo mật

### 1.1 Phân biệt theo các lớp bảo mật

|Lớp hệ thống	|Rủi ro/tiêu chuẩn cần quan tâm|
| :--- | :--- |
|Application	|OWASP Top 10|
|API	|OWASP API Security Top 10|
|Web Server	|CIS Benchmark + OWASP|
|Operating System	|CIS Benchmark|
|Network Firewall	|CIS Benchmark|
|Database	|CIS Benchmark|
|Container	|CIS Benchmark|
|Cloud Infrastructure	|CIS Benchmark|
|Security Monitoring	|SIEM, CIS Controls|

**Ví dụ:** Một hệ thống có Web Application, API Gateway, Firewall, Database và Linux Server

- OWASP kiểm tra API có bị Broken Access Control hay SQL Injection không
- CIS kiểm tra Linux Server có cấu hình SSH an toàn không
- CIS kiểm tra Database có quyền truy cập phù hợp không
- CIS kiểm tra Firewall có bật các cơ chế bảo vệ cần thiết không
- SIEM giám sát và cảnh báo khi xảy ra sự cố

