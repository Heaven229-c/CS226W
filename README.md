# Hướng dẫn cài đặt và cấu hình Snort

## 1. Cài đặt Snort

### Bước 1: Cài đặt các gói phụ thuộc
Trước khi cài đặt Snort, cần cài đặt các gói phụ thuộc sau:
```bash
sudo apt update
sudo apt install -y build-essential libpcap-dev libpcre3-dev libdumbnet-dev bison flex zlib1g-dev
```

### Bước 2: Tải và cài đặt Snort
Tải bản Snort mới nhất từ trang chủ Snort:
```bash
wget https://www.snort.org/downloads/snort/snort-2.9.x.tar.gz
```
Giải nén và cài đặt:
```bash
tar -xvzf snort-2.9.x.tar.gz
cd snort-2.9.x
./configure --enable-sourcefire
make
sudo make install
```

### Bước 3: Tạo thư mục cần thiết
```bash
sudo mkdir -p /etc/snort/rules
sudo mkdir /var/log/snort
sudo touch /etc/snort/rules/local.rules
sudo touch /etc/snort/snort.conf
```
Cấp quyền cho thư mục log:
```bash
sudo chmod -R 5775 /var/log/snort
```

### Bước 4: Kiểm tra cài đặt
Kiểm tra phiên bản Snort:
```bash
snort -V
```

## 2. Cấu hình Snort

### Bước 1: Cấu hình file `snort.conf`
Mở file cấu hình Snort:
```bash
sudo nano /etc/snort/snort.conf
```
Thêm các cài đặt cơ bản:
```bash
# Thư mục chứa các rules
var RULE_PATH /etc/snort/rules

# Thư mục log
var LOG_PATH /var/log/snort

# Cấu hình mạng nội bộ
ipvar HOME_NET 192.168.1.0/24

# Nạp rules
include $RULE_PATH/local.rules
```

### Bước 2: Thêm luật Snort
Chỉnh sửa file `local.rules` để thêm luật mới:
```bash
sudo nano /etc/snort/rules/local.rules
```
Ví dụ, thêm luật phát hiện gói tin ICMP:
```
alert icmp any any -> any any (msg:"ICMP Packet Detected"; sid:1000001; rev:1;)
```

### Bước 3: Kiểm tra cấu hình
Kiểm tra tính đúng của cài đặt:
```bash
snort -T -c /etc/snort/snort.conf
```

### Bước 4: Chạy Snort
Chạy Snort trong chế độ giám sát mạng:
```bash
snort -A console -q -c /etc/snort/snort.conf -i eth0
```
Trong đó:
- `-A console`: Hiển thị cảnh báo trên console.
- `-q`: Chế độ im lặng (giảm log không cần thiết).
- `-i eth0`: Chọn giao diện mạng `eth0`.

