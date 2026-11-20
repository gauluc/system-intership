# 1. Container

Container là một môi trường thực thi ứng dụng được cô lập ở cấp độ hệ điều hành (OS-level isolation)

Container đóng gói:

- Application
- Runtime
- Libraries
- Dependencies
- Configuration cần thiết

Khác với VM, container không cần một Guest OS riêng cho từng ứng dụng. Các container thông thường chia sẻ Kernel của hệ điều hành Host

Ví dụ: Một Linux Server chạy Docker Engine:

```
Physical Server
      |
 Linux Kernel
      |
 Container Runtime
      |
 +----+-----+------+
 |          |      |
 C1         C2     C3
 |          |      |
Nginx     Redis  .NET API
```

Mỗi container có môi trường và filesystem riêng, nhưng cùng sử dụng Linux Kernel của Host

# 2. Container hoạt động như thế nào?

Container trên Linux chủ yếu dựa vào hai cơ chế Kernel:

### 2.1. Namespaces – Cơ chế cô lập

Namespaces giúp các tiến trình trong container nhìn thấy một môi trường hệ thống riêng biệt

Các loại Namespace quan trọng:

|Namespace	|Chức năng|
| :--- | :--- |
|PID	|Cô lập Process ID|
|NET	|Cô lập Network Interface, Routing, Port|
|MNT	|Cô lập Mount Point và Filesystem|
|UTS	|Cô lập Hostname và Domain Name|
|IPC	|Cô lập Inter-process Communication|
|USER	|Cô lập User ID và Group ID|
|CGROUP	|Cô lập chế độ xem cgroup trong hệ thống hiện đại|

### 2.2. Cgroups – Control Groups

Cgroups kiểm soát việc sử dụng tài nguyên của các process

Ví dụ:

Container A:
- CPU Limit: 2 CPU
- Memory Limit: 1 GB

Container B:
- CPU Limit: 1 CPU
- Memory Limit: 512 MB

Cgroups giúp hạn chế mức tiêu thụ tài nguyên

Các tài nguyên thường được kiểm soát:

- CPU
- Memory
- I/O
- Process count
- Một số loại tài nguyên khác tùy phiên bản Kernel và cgroup

### 2.3. Union Filesystem và Container Image

Container Image là mẫu đóng gói ứng dụng

Ví dụ Dockerfile:

```
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

Quy trình:

1. FROM: Chọn Image nền
2. COPY: Sao chép nội dung ứng dụng
3. EXPOSE: Khai báo cổng dự kiến sử dụng
4. CMD: Xác định lệnh khởi chạy

Image thường được xây dựng từ nhiều layer

Khi container được khởi chạy, runtime tạo một writable layer cho container trên các layer Image bất biến

# 3. Docker

**Docker** là một nền tảng giúp xây dựng, phân phối và chạy container

Docker có các thành phần:

1. Docker Client: Là giao diện để người dùng tương tác

2. Docker Daemon: dockerd chịu trách nhiệm quản lý các hoạt động Docker:

- Build Image
- Pull Image
- Create Container
- Manage Network
- Manage Volume

3. Containerd: Containerd là Container Runtime cấp cao, quản lý vòng đời container và các hoạt động như:

- Image management
- Container lifecycle
- Snapshot management
- Runtime integration

4. Runc: Runc là OCI-compatible low-level runtime. Nó thực hiện việc tạo và chạy container thông qua các tính năng của Linux Kernel như namespaces và cgroups

5. Docker Registry: Registry là nơi lưu trữ và phân phối Image

6. Docker Network: Docker cung cấp nhiều mô hình mạng:

|Network	|Chức năng|
| :--- | :--- |
|Bridge	|Mạng ảo cho container trên cùng Host|
|Host	|Container sử dụng Network Namespace của Host|
|None	|Không cấu hình kết nối mạng thông thường|
|Overlay	|Mạng kết nối container qua nhiều Docker Host|
|Macvlan	|Container có MAC riêng trên mạng vật lý|

# 4. So sánh Virtual Machine và Container

|Tiêu chí	|Virtual Machine|	Container|
| :--- | :--- | :--- |
|Công nghệ	|Hardware Virtualization	|OS-level Virtualization|
|Lớp quản lý	|Hypervisor	|Container Runtime|
|Kernel	|Guest Kernel riêng	|Chia sẻ Host Kernel|
|OS	|Guest OS riêng	|Không có Guest OS đầy đủ|
|Khởi động	|Thường lâu hơn	|Thường nhanh hơn|
|Tài nguyên	|Thường lớn hơn	|Thường nhẹ hơn|
|Isolation	|Cô lập ở cấp VM	|Cô lập ở cấp process|
|Portability	|Phụ thuộc Hypervisor và image format	|Phụ thuộc image, runtime và OS compatibility|
|Phù hợp	|Hệ điều hành, workload độc lập	|Microservices, ứng dụng đóng gói|