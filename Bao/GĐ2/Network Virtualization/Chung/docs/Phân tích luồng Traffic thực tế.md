### Một Kubernetes Cluster có hai Node:

```
Node 1: 192.168.1.11
Node 2: 192.168.1.12

Pod-A: 10.10.1.10
Pod-B: 10.10.2.10

Service:
10.96.0.100:80
```

Pod-A gửi HTTP request đến Service

### Nếu sử dụng Cilium

Quy trình:

1. Pod-A phát sinh packet
2. eBPF program trên Node 1 xử lý packet
3. Cilium tra cứu Service VIP trong eBPF map
4. Chọn backend Pod-B
5. Thực hiện chuyển tiếp packet
6. Nếu Pod-B nằm trên Node 2, traffic được định tuyến qua mạng giữa hai Node
7. Hubble có thể quan sát flow

Điểm quan trọng: Cilium không cần chuyển mọi packet qua một SDN Controller tập trung

### Nếu sử dụng Kube-OVN

Quy trình:

1. Pod-A gửi packet
2. Packet đi qua interface của Pod
3. OVS trên Node 1 nhận packet
4. OVN logical flow xử lý traffic
5. Nếu cần, packet được encapsulate vào Geneve
6. OVS trên Node 2 nhận packet
7. Packet được decapsulate và chuyển đến Pod đích

Kube-OVN sử dụng logical network được định nghĩa trong OVN để điều khiển việc forwarding

### Nếu sử dụng OVN độc lập

OVN cũng có thể xây dựng mạng cho VM mà không cần Kubernetes:

```
OpenStack
    |
    v
Neutron OVN Driver
    |
    v
OVN Northbound DB
    |
    v
ovn-northd
    |
    v
OVN Southbound DB
    |
    v
ovn-controller
    |
    v
OVS
    |
    v
VM
```