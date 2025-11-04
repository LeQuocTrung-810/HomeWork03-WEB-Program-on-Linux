Bài tập 3   : môn Phát triển ứng dụng trên nền web  
Giảng viên  : Đỗ Duy Cốp  
Lớp học phần: 58KTPM  
Ngày giao   : 2025-10-24 13:50  
Hạn nộp     : 2025-11-05 00:00  
------------------------------------------------------
Yêu cầu: LẬP TRÌNH ỨNG DỤNG WEB trên nền linux
## 1. Cài đặt môi trường linux: SV chọn 1 trong các phương án
- enable wsl: cài đặt docker desktop  
WSL (Windows Subsystem for Linux) cho phép chạy trực tiếp nhân Linux trên Windows, còn Docker Desktop tận dụng WSL2 để chạy container một cách nhẹ, nhanh và tương thích cao.

a. Nhẹ và hiệu năng cao  

- WSL2 không cần tạo máy ảo riêng biệt như VMware hay VirtualBox → ít tốn RAM và CPU hơn rất nhiều.

- Docker Desktop sử dụng WSL2 backend nên chạy container gần như tốc độ native của Linux.

- Việc khởi động, dừng container chỉ mất vài giây (trong khi VM mất vài phút).

b. Cài đặt đơn giản  
- Vào window menu tìm kiếm CMD và chạy với quyền quản trị viên, sử dụng lệnh: ```wsl --install```

- Docker Desktop cũng tự động kết nối với WSL2 mà không cần cấu hình mạng, IP, bridge như các phần mềm ảo hoá khác.

- Không phải cài nhiều thứ như “ISO Ubuntu, cấu hình ổ đĩa, card mạng” như trong VMware/VirtualBox.

c. Được tích hợp trực tiếp trên windows  
d. Tương thích tuyệt đối với docker  
=> 
