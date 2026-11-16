**DDoS** (Distributed Denial of Service) là hình thức tấn công từ chối dịch vụ phân tán

Kẻ tấn công sử dụng nhiều thiết bị hoặc nguồn phát lưu lượng khác nhau để gửi một lượng lớn traffic đến hệ thống mục tiêu

Mục tiêu là làm cạn kiệt tài nguyên khiến người dùng hợp pháp không thể truy cập dịch vụ

**Anti-DDoS Protection** là nhóm giải pháp giúp phát hiện, phân tích, hấp thụ và giảm thiểu lưu lượng tấn công DDoS để đảm bảo dịch vụ vẫn hoạt động bình thường

**Anti-DDoS** không chỉ đơn giản là chặn IP

Nó phải phân tích:

- Lưu lượng mạng
- Tốc độ tăng traffic
- Số lượng kết nối
- Loại giao thức
- Hành vi truy cập
- Tính hợp lệ của request
- Mức độ bất thường so với Baseline

# 1. Các loại tấn công DDoS

## 1.1 Volumetric Attacks

Đây là loại tấn công nhằm chiếm dụng băng thông mạng bằng cách tạo ra lượng traffic rất lớn

Một số hình thức:

1. UDP Flood

Kẻ tấn công gửi lượng lớn UDP Packet đến Server

Server phải xử lý lượng lớn packet, gây tiêu tốn tài nguyên

2. ICMP Flood

Gửi số lượng lớn ICMP Echo Request đến mục tiêu

Mục đích làm tăng tải xử lý và chiếm băng thông

3. DNS Amplification

Kẻ tấn công lợi dụng DNS Server để khuếch đại lượng traffic gửi đến nạn nhân

Cơ chế:

```
Attacker
   |
   | DNS Query
   | Spoofed Source IP = Victim
   v
Open DNS Resolver
   |
   | Large DNS Response
   v
Victim Server
```

Kích thước response có thể lớn hơn nhiều so với request ban đầu

## 1.2 Protocol Attacks

Nhóm này khai thác cách các giao thức mạng xử lý kết nối

1. SYN Flood

TCP sử dụng quá trình bắt tay ba bước:

```
Client                 Server
  |                       |
  |-------- SYN --------->|
  |                       |
  |<----- SYN-ACK --------|
  |                       |
  |        ACK            |
  |---------------------->|
```

Trong SYN Flood:

- Attacker gửi nhiều SYN
- Server phản hồi SYN-ACK
- Attacker không hoàn thành bước ACK
- Server phải duy trì nhiều kết nối chưa hoàn tất

Kết quả: SYN Backlog hoặc tài nguyên kết nối bị quá tải

2. ACK Flood

Attacker gửi lượng lớn TCP ACK Packet khiến thiết bị phải tiêu tốn tài nguyên xử lý và kiểm tra trạng thái kết nối

3. Fragmentation Attacks

Kẻ tấn công lợi dụng việc phân mảnh IP để tạo các packet bất thường hoặc làm tăng chi phí xử lý và tái lắp ráp

## 1.3 Application Layer Attacks (Layer 7)

Đây là nhóm tấn công nhắm vào ứng dụng.

Một số hình thức:

1. HTTP Flood

Gửi hàng loạt HTTP Request đến Web Server

Mục tiêu là làm quá tải Web Server, API Gateway hoặc Backend

2. Slowloris

Attacker mở nhiều HTTP Connection nhưng gửi request rất chậm hoặc không hoàn tất request

Server phải giữ kết nối trong thời gian dài, gây cạn kiệt tài nguyên

3. DNS Query Flood

Gửi lượng lớn truy vấn DNS để làm quá tải DNS Server

4. Login/API Flood

Gửi lượng lớn request đến endpoint đăng nhập hoặc API tốn tài nguyên

Loại tấn công này có thể khiến Database, Authentication Service hoặc Backend bị quá tải dù băng thông tổng thể chưa quá lớn

# 2. Các chức năng chính của Anti-DDoS

## 2.1 Traffic Monitoring

Giám sát lưu lượng mạng theo thời gian thực

Các thông số:

- Incoming Traffic
- Outgoing Traffic
- Packets Per Second (PPS)
- Bits Per Second (BPS)
- Requests Per Second (RPS)
- Concurrent Connections
- TCP SYN Rate
- UDP Traffic
- ICMP Traffic

## 2.2 Anomaly Detection

Phát hiện lưu lượng bất thường dựa trên Baseline

Hệ thống nhận diện mức tăng bất thường và có thể kích hoạt cơ chế giảm thiểu

Một số phương pháp:

- Threshold-based Detection
- Statistical Analysis
- Behavioral Analysis
- Machine Learning
- Signature-based Detection

## 2.3 SYN Flood Protection

Các biện pháp có thể sử dụng:

- SYN Cookies
- SYN Proxy
- TCP Connection Validation
- SYN Rate Limiting
- Connection Timeout
- Backlog Protection

SYN Proxy có thể đứng giữa Client và Server để xác minh quá trình bắt tay TCP trước khi thiết lập kết nối đến Backend

## 2.4 UDP/ICMP Flood Protection

Chức năng:

- Giới hạn PPS
- Rate Limiting
- Packet Filtering
- Protocol Validation
- ACL
- Source Reputation
- Chặn lưu lượng không cần thiết

## 2.5 HTTP Flood Protection

Anti-DDoS tầng ứng dụng có thể kết hợp:

- HTTP Request Rate Limiting
- IP Reputation
- Bot Detection
- CAPTCHA
- JavaScript Challenge
- Behavioral Analysis
- Session Validation

## 2.6 Traffic Scrubbing

Traffic Scrubbing là quá trình đưa lưu lượng đi qua hệ thống lọc chuyên dụng để loại bỏ traffic độc hại

Quy trình:

```
           INTERNET
               |
               v
        ATTACK TRAFFIC
               |
               v
       +---------------+
       | SCRUBBING     |
       | CENTER        |
       +---------------+
               |
         Traffic Analysis
               |
        +------+------+
        |             |
        v             v
    Malicious       Legitimate
     Traffic         Traffic
        |             |
        v             v
      DROP          FORWARD
                      |
                      v
                 TARGET SERVER
```

Scrubbing Center có thể sử dụng:

- Signature Detection
- Rate Limiting
- Protocol Validation
- IP Reputation
- Behavioral Analysis
- Traffic Filtering

## 2.7 Blackhole Routing

Blackhole Routing là kỹ thuật định tuyến lưu lượng đến một địa chỉ hoặc tuyến đường bị loại bỏ

- Ưu điểm: Có thể ngăn traffic tấn công tiếp tục đi vào hạ tầng cần bảo vệ

- Nhược điểm: Traffic hợp pháp đến IP đó cũng bị loại bỏ

Do đó, đây là biện pháp cuối cùng trong một số tình huống, không phải giải pháp tối ưu để duy trì dịch vụ

## 2.8 BGP Flowspec

BGP Flow Specification cho phép phân phối các quy tắc lọc traffic thông qua BGP

BGP Flowspec có thể giúp các Router triển khai rule giảm thiểu DDoS ở nhiều điểm trong mạng

Tuy nhiên, phải kiểm soát chặt việc quảng bá và áp dụng rule để tránh ảnh hưởng lưu lượng hợp pháp

## 2.9 CDN Integration

CDN (Content Delivery Network) phân phối nội dung thông qua nhiều điểm hiện diện (PoP)

Kết hợp CDN và Anti-DDoS giúp:

- Phân tán tải
- Cache nội dung
- Giảm tải Origin Server
- Lọc traffic tại biên mạng
- Giảm tác động của nhiều cuộc tấn công DDoS

CDN không tự động bảo vệ mọi loại traffic, đặc biệt đối với dịch vụ không phải HTTP/HTTPS hoặc các dịch vụ cần truy cập trực tiếp Origin

# 3. Phân biệt các cấp độ Anti-DDoS

|Loại	|Bảo vệ	|Ví dụ|
| :--- | :--- | :--- |
|L3 Protection	|Network Layer	|IP Flood|
|L4 Protection	|Transport Layer	|SYN Flood, UDP Flood|
|L7 Protection	|Application Layer	|HTTP Flood, API Flood|

Điểm quan trọng:

- L3/L4 tập trung bảo vệ mạng, giao thức và kết nối
- L7 tập trung bảo vệ dịch vụ, ứng dụng và logic HTTP
- Một hệ thống Anti-DDoS đầy đủ thường cần phối hợp cả ba lớp

# 4. Ưu và nhược điểm Anti-DDoS

Ưu điểm:

- Duy trì tính sẵn sàng của dịch vụ
- Giảm nguy cơ quá tải Server
- Bảo vệ băng thông mạng
- Phát hiện traffic bất thường
- Giảm thiểu nhiều loại tấn công L3/L4/L7
- Hỗ trợ Traffic Scrubbing

Nhược điểm:

- Cần hạ tầng có năng lực xử lý lớn
- Khó phân biệt traffic hợp pháp và độc hại trong một số trường hợp
- Có thể gây chặn nhầm
- Giải pháp On-premise có giới hạn về băng thông và năng lực thiết bị
