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

2. etcd

etcd là cơ sở dữ liệu key-value phân tán dùng để lưu trữ trạng thái của Kubernetes

Nó lưu thông tin như:

- Pod
- Deployment
- Service
- ConfigMap
- Secret
- Node
- Namespace
- Cluster configuration

Trong môi trường Production, etcd cần:

- Backup thường xuyên
- Bảo vệ dữ liệu
- Mã hóa khi cần
- Triển khai HA với số lượng member phù hợp, thường là 3 hoặc 5
- Giám sát quorum, latency và dung lượng

3. kube-scheduler

Scheduler chịu trách nhiệm lựa chọn Worker Node để chạy Pod chưa được phân bổ

Scheduler xem xét:

- CPU
- Memory
- Node Affinity
- Pod Affinity
- Taints và Tolerations
- Topology Spread
- Volume Constraints
- Các chính sách scheduling khác

4. kube-controller-manager

Đây là thành phần chạy các Controller để duy trì trạng thái mong muốn

Một số Controller:

|Controller	|Chức năng|
| :--- | :--- |
|Node Controller	|Theo dõi trạng thái |
|ReplicaSet Controller	|Duy trì số lượng Pod|
|Deployment Controller	|Quản lý Deployment|
|Job Controller	|Quản lý Job|
|EndpointSlice Controller	|Quản lý EndpointSlice|
|Namespace Controller	|Quản lý vòng đời Namespace|

5. Cloud Controller Manager

Thành phần tích hợp Kubernetes với Cloud Provider

Ví dụ:

- Tạo Cloud Load Balancer
- Quản lý Node liên quan đến hạ tầng Cloud
- Quản lý một số loại Route

Trong môi trường tự triển khai, có thể sử dụng Cloud Controller Manager của nền tảng tương ứng hoặc không triển khai thành phần này nếu không cần

### 2.2 Worker Node

Worker Node là máy chủ thực thi ứng dụng

Một Worker Node có những thành phần chính

1. kubelet

Kubelet là Agent chạy trên mỗi Worker Node

Nhiệm vụ:

- Nhận thông tin Pod được giao
- Kiểm tra trạng thái container
- Gọi Container Runtime
- Đảm bảo container chạy theo cấu hình Pod
- Báo cáo trạng thái về API Server

2. Container Runtime

Container Runtime chịu trách nhiệm chạy container

Ví dụ:

- containerd
- CRI-O

Kubernetes giao tiếp với Container Runtime thông qua CRI (Container Runtime Interface)

Một điểm rất quan trọng: Docker Engine không còn là Container Runtime trực tiếp được Kubernetes hỗ trợ mặc định thông qua dockershim từ Kubernetes 1.24 trở đi. Docker-built images vẫn có thể chạy trên các runtime tương thích OCI như containerd

3. kube-proxy

kube-proxy thực hiện các chức năng mạng cho Service trong nhiều cấu hình Kubernetes

Thông thường nó sử dụng:

- iptables
- IPVS

Nó thiết lập các quy tắc để chuyển traffic đến các Pod phù hợp

Lưu ý: Một số CNI như Cilium có thể thay thế kube-proxy bằng cơ chế eBPF

4. CNI Plugin

CNI (Container Network Interface) là giao diện và hệ sinh thái plugin cấu hình mạng cho container

Ví dụ:

- Calico
- Flannel
- Cilium
- OVN-Kubernetes
- Kube-OVN

CNI có nhiệm vụ:

- Cấp IP cho Pod
- Kết nối Pod với Network
- Hỗ trợ routing
- Hỗ trợ Network Policy tùy plugin
- Hỗ trợ các mô hình Overlay hoặc Routing