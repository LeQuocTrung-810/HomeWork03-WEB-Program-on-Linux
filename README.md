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
=> Lựa chọn phương án  Enable WSL và cài đặt Docker Desktop vì đây là cách nhanh nhất, nhẹ nhàng và dễ sử dụng nhất để tạo môi trường linux trên windows  
## 2. Cài đặt Docker desktop
Truy cập đường link : ```https://docs.docker.com/desktop/setup/install/windows-install/``` để tải và cài docker về máy
## 3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: 
Vào File Explorer tạo 1 folder có tên bất kì (EX: MyWebApp) để chứa file docker-compose.yml.
có thể khởi tạo các container thông qua Visual Studio Code 
```
version: "3.9"

services:
  mariadb:
    image: mariadb:latest
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
    ports:
      - "3306:3306"
    volumes:
      - ./db_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    restart: always
    environment:
      PMA_HOST: mariadb
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8080:80"
    depends_on:
      - mariadb

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: always
    ports:
      - "1880:1880"
    volumes:
      - ./nodered_data:/data
    depends_on:
      - mariadb

  influxdb:
    image: influxdb:latest
    container_name: influxdb
    restart: always
    ports:
      - "8086:8086"
    environment:
      - INFLUXDB_DB=mydb
      - INFLUXDB_ADMIN_USER=admin
      - INFLUXDB_ADMIN_PASSWORD=admin
    volumes:
      - ./influx_data:/var/lib/influxdb

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: always
    ports:
      - "3000:3000"
    depends_on:
      - influxdb

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - "81:80"
      - "443:443"
    volumes:
      - ./html:/usr/share/nginx/html
      - ./nginx.conf:/etc/nginx/conf/nginx.conf
    depends_on:
      - phpmyadmin
      - nodered
```
Sau khi lưu file, ta vào terminal trong VSCode chạy file với dòng lệnh ```docker compose up -d```
Sau khi chạy ta được:
<img width="1107" height="733" alt="image" src="https://github.com/user-attachments/assets/17f3c01a-e1ef-4e72-a74c-4addd9ec7324" />
Sau khi chạy code sẽ tạo ra các container trong Docker desktop  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3c198ad8-d6f0-4c9d-8efa-d9c07b853d88" />
Truy cập http://localhost:1880   
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/47f9f423-c29d-497b-89cc-c06aeec7b504" />
Truy cập http://localhost:3000   
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/19207e04-39f9-4d33-8b1a-e7281aa84f2d" />
Truy cập http://localhost:8080   
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e8a02169-ce4f-40d6-be58-61ee00a9461c" />
Truy cập http://localhost:81  
<img width="1919" height="1036" alt="image" src="https://github.com/user-attachments/assets/82d257fb-290c-4fcd-bf32-feb443291cad" />

## 4. Lập trình front-end và back-end (Web IoT)
Mục tiêu:

- Tạo 1 web iot giám sát nhiệt độ và độ ẩm

- Node-RED sinh dữ liệu cảm biến (giả lập).

- Node-RED lưu vào InfluxDB để hiển thị biểu đồ.

- Frontend index.html gọi API từ Node-RED, hiển thị thông tin hiện tại.

- Grafana vẽ biểu đồ trực quan từ dữ liệu InfluxDB.

## 5. Nginx làm web-server

- Cấu hình nginx để chạy được website qua url http://fullname.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)

- Cấu hình nginx để http://fullname.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)

- Cấu hình nginx để http://fullname.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)
