# 1. Kubernetes

Kubernetes (K8s) là nền tảng mã nguồn mở dùng để tự động hóa việc triển khai, quản lý và mở rộng ứng dụng được container hóa

Kubernetes giải quyết những vấn đề phát sinh khi hệ thống chạy hàng trăm hoặc hàng nghìn container

Ví dụ: Một hệ thống thương mại điện tử có các dịch vụ:

- Identity API
- User API
- Product API
- Order API
- Payment API
- Notification API
- API Gateway
- Redis
- RabbitMQ
- PostgreSQL

Nếu chạy bằng Docker thủ công, quản trị viên phải tự xử lý:

- Container nào chạy trên Server nào?
- Nếu Server bị lỗi thì sao?
- Nếu container bị crash thì ai khởi động lại?
- Khi có nhiều request thì tăng số lượng container như thế nào?
- Các container giao tiếp với nhau ra sao?
- Cập nhật phiên bản mới thế nào mà không gián đoạn dịch vụ?

Kubernetes tự động hóa những công việc này

# 2. Kiến trúc Kubernetes Cluster

Một Kubernetes Cluster bao gồm hai thành phần chính:

1. Control Plane
2. Worker Nodes

### 2.1 Control Plane

Control Plane là bộ não điều khiển Kubernetes Cluster

Các thành phần quan trọng

1. kube-apiserver

Đây là trung tâm giao tiếp của Kubernetes

Mọi yêu cầu quản lý Cluster đều đi qua API Server

