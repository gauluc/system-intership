# 1. Máy ảo (Virtual Machine)

**VM** là một máy tính được mô phỏng bằng phần mềm, có tài nguyên phần cứng ảo và hệ điều hành riêng.

Một VM thường gồm:

|Thành phần	|Chức năng|
| :--- | :--- |
|vCPU	|CPU ảo|
|vRAM	|Bộ nhớ ảo|
|vDisk	|Ổ cứng ảo|
|vNIC	|Card mạng ảo|
|BIOS/UEFI	|Firmware ảo|
|Virtual Devices	|Thiết bị ngoại vi ảo|

### 1.1 CPU Virtualization

CPU Virtualization cho phép nhiều VM chia sẻ CPU vật lý

Ví dụ: Server có 16 CPU Core vật lý

Ta cấp:

- VM1: 4 vCPU
- VM2: 4 vCPU
- VM3: 8 vCPU
- VM4: 8 vCPU

Tổng số vCPU = 24, lớn hơn 16 CPU Core vật lý

Đây gọi là CPU Overcommitment

Điều đó không có nghĩa 24 vCPU có thể thực thi đồng thời trên 16 core vật lý. Hypervisor sẽ lập lịch cho các vCPU sử dụng tài nguyên CPU

Các khái niệm cần biết:

- vCPU Scheduling: Hypervisor quyết định vCPU nào sẽ chạy trên pCPU nào và khi nào, tương tự như bộ lập lịch của hệ điều hành nhưng ở tầng ảo hóa
- CPU Overcommitment: Là việc cấp nhiều vCPU hơn số lượng pCPU thực tế
- CPU Pinning: Gán vCPU cố định vào một hoặc nhiều pCPU cụ thể, tránh scheduler tự động di chuyển
- NUMA: Kiến trúc trong đó mỗi socket CPU có vùng RAM riêng, truy cập RAM “local” nhanh hơn RAM “remote”
- CPU Steal Time: Thời gian vCPU muốn chạy nhưng bị hypervisor “cướp” CPU để phục vụ workload khác
- CPU Ready Time: Thời gian vCPU phải chờ để được chạy trên pCPU, dù đã sẵn sàng

### 1.2. Memory Virtualization

Memory Virtualization cho phép Hypervisor phân phối RAM vật lý cho nhiều VM

Ví dụ: Server có 128 GB RAM

Physical RAM = 128 GB

- VM1 = 32 GB
- VM2 = 32 GB
- VM3 = 32 GB
- VM4 = 32 GB


**Một số cơ chế quan trọng:**

- Memory Overcommitment: Cấp tổng RAM ảo vượt RAM vật lý, dựa trên mức sử dụng và cơ chế quản lý bộ nhớ
- Ballooning: Thu hồi một phần RAM từ VM thông qua balloon driver để phân phối lại
- Memory Swapping: Chuyển dữ liệu bộ nhớ sang disk khi thiếu RAM, thường gây giảm hiệu năng đáng kể
- Huge Pages: Sử dụng các trang bộ nhớ lớn hơn trang thông thường để giảm chi phí quản lý bộ nhớ trong một số workload

### 1.3. Disk Virtualization

VM không nhất thiết sử dụng ổ cứng vật lý riêng

Thông thường VM sử dụng file hoặc logical volume đại diện cho ổ cứng

Các định dạng phổ biến:

|Định dạng	|Nền tảng|
| :--- | :--- |
|QCOW2	|QEMU/KVM|
|RAW	|QEMU/KVM|
|VMDK	|VMware|
|VHD/VHDX	|Hyper-V|
|VDI	|VirtualBox|

Một số khái niệm:

- Thin Provisioning: Cấp dung lượng logic lớn hơn dung lượng thực tế đã sử dụng
- Thick Provisioning: Dành trước dung lượng lưu trữ
- Snapshot: Lưu trạng thái hoặc thay đổi của disk tại một thời điểm
- Cloning: Sao chép VM hoặc disk
- Live Migration: Di chuyển VM đang chạy giữa các host nếu hạ tầng đáp ứng điều kiện

### 1.4. Network Virtualization trong VM

Mỗi VM có thể có một hoặc nhiều vNIC

vNIC được kết nối vào virtual switch

