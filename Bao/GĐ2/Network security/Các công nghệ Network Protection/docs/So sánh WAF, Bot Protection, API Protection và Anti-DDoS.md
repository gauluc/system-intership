# 1. So sánh tổng thể

|Tiêu chí	|WAF	|Bot Protection	|API Protection	|Anti-DDoS|
| :--- | :--- | :--- | :--- | :--- |
|Mục tiêu	|Bảo vệ Web	|Kiểm soát Bot	|Bảo vệ API	|Duy trì dịch vụ|
|SQL |Injection	|Có	|Không phải chức năng chính	|Có thể có	|Không|
|XSS	|Có	|Không	|Có thể có	|Không|
|Brute Force	|Có thể	|Có	|Có	|Không phải chức năng chính|
|Bot Detection	|Hạn chế	|Có	|Có thể	|Có thể|
|JWT Validation	|Tùy sản phẩm	|Không	|Có	|Không|
|Schema Validation	|Có thể	|Không	|Có	|Không|
|Rate Limiting	|Có	|Có	|Có	|Có
|SYN Flood	|Không	|Không	|Không	|Có|
|UDP Flood	|Không	|Không	|Không	|Có|
|HTTP Flood	|Có thể	|Có	|Có thể	|Có|
|Traffic Scrubbing	|Không	|Không	|Không	|Có|

# 2. Mối quan hệ giữa các chức năng

Một kiến trúc bảo mật ứng dụng có thể triển khai như sau:

```
                     INTERNET
                         |
                         v
                +------------------+
                |   ANTI-DDoS      |
                |   L3/L4/L7       |
                +------------------+
                         |
                         v
                +------------------+
                |       CDN        |
                +------------------+
                         |
                         v
                +------------------+
                |       WAF        |
                | SQLi / XSS / RCE  |
                +------------------+
                         |
                         v
                +------------------+
                |  BOT PROTECTION  |
                | Bot Detection    |
                | CAPTCHA          |
                +------------------+
                         |
                         v
                +------------------+
                |   API GATEWAY    |
                +------------------+
                         |
                +--------+--------+
                |                 |
                v                 v
          Authentication    Rate Limiting
                |                 |
                +--------+--------+
                         |
                         v
                +------------------+
                |  API PROTECTION  |
                | Schema Validation|
                | Access Control   |
                | Threat Detection |
                +------------------+
                         |
                         v
                +------------------+
                | BACKEND SERVICES |
                +------------------+
                         |
              +----------+----------+
              |          |          |
              v          v          v
           Users API  Firewall API  Orders API
```
