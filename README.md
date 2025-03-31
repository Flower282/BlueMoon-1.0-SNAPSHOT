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
docker pull ghcr.io/flower282/bluemoon:1.0
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
```
export DISPLAY=$(ip route | awk '/default/ {print $3}'):0
```  
Chạy container với biến môi trường DISPLAY:
```
docker run --name bluemoon-container -e DISPLAY=$DISPLAY -d ghcr.io/flower282/bluemoon:1.0
```  
**Giải thích lệnh**:  
-    `-e DISPLAY=$DISPLAY` → Truyền biến môi trường để container có thể kết nối X server.
-    `-d` → Chạy container ở chế độ nền.
Kiểm tra container đang chạy:
```
docker ps
```
Nếu thấy container `bluemoon-container` trong danh sách, bạn đã chạy thành công!
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






