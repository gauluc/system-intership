# 1. CNI là gì?

CNI (Container Network Interface) là một đặc tả (specification) xác định cách container runtime hoặc hệ thống quản lý container gọi network plugin để cấu hình mạng cho container

CNI không phải là một giải pháp mạng cụ thể

CNI là giao diện chuẩn hóa để các giải pháp mạng khác nhau có thể tích hợp vào Kubernetes

Ví dụ các CNI plugin:

- Cilium
- Kube-OVN
- Calico
- Flannel
- Weave Net

## 2. Tại sao cần CNI?

Kubernetes quản lý Pod, nhưng bản thân Kubernetes không trực tiếp triển khai toàn bộ Data Plane để kết nối các Pod

Kubernetes cần một giải pháp mạng bên ngoài để thực hiện:

- Cấp IP cho Pod
- Tạo network interface
- Thiết lập routing
- Kết nối Pod trong cùng node
- Kết nối Pod giữa các node
- Thực hiện Network Policy
- Kết nối Pod với các mạng bên ngoài

CNI giúp chuẩn hóa quá trình này

### Nếu không có CNI?

Giả sử Kubernetes tạo một Pod:

```
Pod-A
  |
  v
Container Runtime
```

Pod có thể được tạo thành công nhưng chưa có network interface được cấu hình đúng, không có IP và không thể giao tiếp với các Pod khá

### Khi có CNI

```
Kubernetes
    |
    v
Kubelet
    |
    v
Container Runtime
    |
    v
CNI Plugin
    |
    +---- Allocate IP
    |
    +---- Create Network Interface
    |
    +---- Configure Routes
    |
    +---- Connect Pod to Network
```

Khi Pod được tạo, runtime gọi CNI để thiết lập mạng. Khi Pod bị xóa, runtime gọi CNI để giải phóng tài nguyên mạng

## 3. Quy trình CNI hoạt động

Giả sử tạo Pod: `kubectl run nginx --image=nginx`

Quy trình đơn giản:

Bước 1: Kubernetes Scheduler chọn Node

Bước 2: Kubelet trên Node yêu cầu container runtime tạo Pod sandbox

Bước 3: Container runtime gọi CNI plugin với lệnh ADD

Bước 4: CNI thực hiện:

- Cấp IP
- Tạo interface
- Cấu hình network namespace
- Cấu hình routing
- Kết nối interface vào Data Plane

Bước 5: Pod có thể giao tiếp với các workload khác

Khi xóa Pod, runtime gọi CNI DEL để dọn dẹp

## 4. Các thao tác CNI thường gặp

| Operation | Chức năng |
| :--- | :--- |
| ADD | Thiết lập mạng cho container |
| DEL | Xóa cấu hình mạng |
| CHECK | Kiểm tra cấu hình mạng |
| VERSION | Tra cứu phiên bản CNI |

Lưu ý: CNI specification không quy định bắt buộc mọi plugin phải tự triển khai toàn bộ IPAM, Routing, Firewall hay Load Balancer. Những chức năng đó phụ thuộc vào giải pháp CNI