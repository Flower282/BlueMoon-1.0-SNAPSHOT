# BlueMoon-1.0-SNAPSHOT: Phần mềm quản lý và thu phí ở khu chung cư
## Giới thiệu
Đây là hướng dẫn giúp bạn tải Docker, lấy image từ GitHub Container Registry (GHCR) và chạy container từ image bluemoon:1.0.
## Cài đặt Docker
Nếu bạn chưa có Docker, hãy tải và cài đặt theo hướng dẫn dưới đây:  
-    **Windows & Mac:** Tải từ [Docker Desktop](https://www.docker.com/products/docker-desktop/) và cài đặt theo hướng dẫn.
-    **Linux (Ubuntu/Debian):**
```
    sudo apt update
    sudo apt install -y docker.io
```
&nbsp;&nbsp;&nbsp;&nbsp;Kiểm tra Docker đã cài đặt thành công chưa:  
```
    docker --version
```
&nbsp;&nbsp;&nbsp;&nbsp;Nếu thấy phiên bản hiển thị, bạn đã cài đặt thành công.  
## Lấy image từ GHCR  
Chạy lệnh sau để tải image `bluemoon:1.0` về máy:
```
docker pull ghcr.io/flower282/bluemoon-app:1.0@sha256:add812071818e9aac169b7eb487b2acc303d8e1599895380b3e5e514d97df01a
```  
Kiểm tra image đã tải về:  
```
docker images
```  
Nếu thấy `ghcr.io/flower282/bluemoon` xuất hiện trong danh sách, bạn đã tải thành công.  
## Chạy container từ image  
Ứng dụng này yêu cầu **VcXsrv** (hoặc X11 server tương tự) để hiển thị giao diện đồ họa.
### 1. Cài đặt X11 server  
**Windows**: Cài đặt VcXsrv từ [link này](https://vcxsrv.com/) và cài đặt.  
-    Chạy **XLaunch**, chọn **Multiple windows**, và đặt **Display number = 0**.
-    Tick **Disable Access Control** để container có thể kết nối.

**MacOS**: Cài đặt **XQuartz**:
```
brew install --cask xquartz
```
-    Chạy **XQuartz**, vào Preferences > Security > Tick **Allow connections from network clients**.

-    Khởi động lại XQuartz.

**Linux**: Nếu bạn sử dụng môi trường đồ họa X11, chỉ cần đảm bảo X11 đã được cài đặt.
### 2. Chạy container với hỗ trợ GUI  
Chạy lệnh sau để cho phép container kết nối với X server:
**Windows (VcXsrv)**:
```
set DISPLAY=host.docker.internal:0.0
```
**Mac/Linux**:  
Cài biến môi trường:
```
export DISPLAY=$(ip route | awk '/default/ {print $3}'):0
```  
Chạy container với biến môi trường DISPLAY:
```
sudo docker run --name bluemoon-container \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -d ghcr.io/flower282/bluemoon-app:1.0@sha256:add812071818e9aac169b7eb487b2acc303d8e1599895380b3e5e514d97df01a

```  
**Giải thích lệnh**:  
-    `-e DISPLAY=$DISPLAY` → Truyền biến môi trường để container có thể kết nối X server.
-    `-d` → Chạy container ở chế độ nền.
Kiểm tra container đang chạy:
```
docker ps
```
Nếu thấy container `bluemoon-container` trong danh sách, bạn đã chạy thành công!  
## Do yêu cầu bảo mật và xác thực thông tin, ứng dụng không cho phép tự tạo tài khoản, bạn có thể dùng tài khoản sau để đăng nhập:  
username: `admin@gmail.com`  
password: `admin`  
## Dừng & Xóa container  
Nếu bạn muốn dừng container đang chạy:  
```
docker stop bluemoon-container
```  
Xóa container (sau khi đã dừng):  
```
docker rm bluemoon-container
```  
Xóa image nếu không cần dùng nữa:
```
docker rmi ghcr.io/flower282/bluemoon:1.0
```
## Thông tin thêm  
Nếu có bất kỳ vấn đề gì, bạn có thể kiểm tra log container bằng lệnh:  
```
docker logs bluemoon-container
```  
Hoặc kiểm tra lỗi Docker:
```
sudo systemctl status docker
```  
Nếu bạn gặp lỗi, vui lòng liên hệ hoặc tạo **issue** trên repository GitHub.  
  
*Dự án vẫn đang tiếp tục phát triển nên có nhiều chức năng còn chưa hoàn thiện, hãy thường xuyên cập nhật để có thể trải nghiệm đầy đủ chức năng nhé!*






