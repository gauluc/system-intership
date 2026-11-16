**Antivirus (AV)** là phần mềm bảo mật được thiết kế để phát hiện, ngăn chặn, cách ly và loại bỏ virus cũng như các loại mã độc (Malware) trên thiết bị đầu cuối

Antivirus thường được cài đặt trực tiếp trên:

- Máy tính Windows, Linux, macOS
- Máy chủ vật lý
- Máy chủ ảo (VM)
- Một số môi trường máy chủ doanh nghiệp

# 1. Antivirus hoạt động như thế nào

Antivirus thường thực hiện theo quy trình:

![image](../image/1.png)

**Ví dụ:**

Một người dùng tải file malware.exe từ Internet

1. Người dùng tải file về máy
2. Antivirus kiểm tra nội dung file
3. Antivirus so sánh chữ ký mã độc và phân tích hành vi
4. Phát hiện file chứa mã độc
5. Chặn thực thi và đưa file vào vùng cách ly (Quarantine)
6. Ghi nhận sự kiện bảo mật

## 2. Các phương pháp phát hiện mã độc

### 2.1 Signature-based Detection

Phát hiện dựa trên chữ ký mã độc đã biết

Mỗi mã độc có thể có những đặc điểm nhận dạng như:

- Hash MD5, SHA-256
- Byte pattern
- Chuỗi lệnh đặc trưng
- Cấu trúc file độc hại


