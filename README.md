# Hướng dẫn Package ROS Noetic cho Raspberry Pi 3 và Laptop

Package này hỗ trợ thiết lập ROS Noetic phân tán: **Laptop** chạy `roscore` và `teleop` (điều khiển bàn phím), **Raspberry Pi 3** chạy camera và động cơ.  
Làm theo hướng dẫn dưới để clone và sử dụng.

---

## Yêu cầu

**Laptop:**
- Ubuntu 20.04 với ROS Noetic (Cài đặt ROS Noetic).

**Raspberry Pi 3:**
- Ubuntu 20.04 (hoặc Raspbian với ROS Noetic desktop).
- ROS Noetic đã cài.
- Camera USB (Camera V2.1).
- Động cơ Dynamixel kết nối với OPENRB 150
- Gói: `dynamixel_sdk`, `dynamixel_sdk_examples`.

**Mạng:**  
Cả hai thiết bị cùng mạng Wi-Fi.

**Workspace:**  
Không gian làm việc ROS (ví dụ: `~/catkin_ws`) trên cả hai thiết bị.

---

## Cài đặt

### Clone Repository
Trên **cả laptop và Raspberry Pi 3**, clone package vào workspace ROS:

```bash
cd ~/catkin_ws/src
git clone https://github.com/EBang2k4/DynamixelSDK.git
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

---

### Cài đặt Dependencies
Trên **cả hai thiết bị**, cài các gói cần thiết:

```bash
sudo apt update
sudo apt install ros-noetic-teleop-twist-keyboard ros-noetic-dynamixel-sdk ros-noetic-dynamixel-workbench
```

---

### Cấu hình Môi trường Mạng

- Trên laptop, tìm địa chỉ IP:

```bash
ifconfig
```

Ghi lại IP (ví dụ: `192.168.1.100`).

- Trên **cả hai thiết bị**, thêm biến môi trường vào `~/.bashrc`:

```bash
echo "export ROS_MASTER_URI=http://192.168.1.100:11311" >> ~/.bashrc
echo "export ROS_HOSTNAME=your_device_ip" >> ~/.bashrc
source ~/.bashrc
```

**Lưu ý:** Thay `your_device_ip` bằng IP của laptop hoặc Pi 3 tương ứng.

---

## Sử dụng

### Trên Laptop:

- Chạy `roscore`:

```bash
roscore
```

- Mở terminal mới, chạy `teleop` để điều khiển:

```bash
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```

---

### Trên Raspberry Pi 3:

- Chạy node camera để phát video:

```bash
rosrun uvc_camera uvc_camera_node
```

- Chạy node điều khiển động cơ (giả sử package có node `motor_control`):

```bash
rosrun your_package_name motor_control
```

---

### Kiểm tra:

- Trên laptop, xem video từ camera:

```bash
rosrun image_view image_view image:=/camera/image_raw
```

- Dùng phím (theo hướng dẫn `teleop_twist_keyboard`) để điều khiển động cơ trên Pi 3.

---

## Lưu ý

- Thay `your_username/your_repository` và `your_package_name` bằng thông tin thực tế.
- Đảm bảo camera và động cơ được kết nối đúng (xem tài liệu trong package).
- Nếu gặp lỗi mạng, kiểm tra IP và firewall (cổng `11311`).

---

## Hỗ trợ

- Xem thêm: [ROS Wiki](http://wiki.ros.org/).
- Báo lỗi tại: Issues trên GitHub Repository của bạn.

