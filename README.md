# Hướng dẫn Cài đặt và Cấu hình Snort

## Bước 1: Cập nhật Danh sách Gói
Trước tiên, chúng ta cần cập nhật danh sách các gói trên hệ điều hành Ubuntu bằng lệnh sau:

```bash
sudo apt-get update
```

## Bước 2: Cài đặt Snort
Việc cài đặt Snort trên Ubuntu rất đơn giản, chỉ cần sử dụng lệnh sau:

```bash
sudo apt-get install snort -y
```

Trong quá trình cài đặt, hệ thống sẽ yêu cầu bạn chọn giao diện mạng mà Snort sẽ giám sát.

Để xác định giao diện mạng, sử dụng lệnh sau:

```bash
ip a
```

Sau đó, nhập giao diện mạng tương ứng vào phần cấu hình của Snort.

Tiếp theo, Snort sẽ yêu cầu bạn xác định giá trị `HOME_NET`. Trong bài hướng dẫn này, Snort được sử dụng như một Host Based IDS, vì vậy bạn chỉ cần nhập địa chỉ IP của máy Ubuntu.

Để kiểm tra phiên bản Snort đã cài đặt, sử dụng lệnh sau:

```bash
snort -V
```

## Bước 3: Cấu hình Snort
File cấu hình `snort.conf` được lưu tại thư mục `/etc/snort`. Snort sẽ hoạt động dựa trên cấu hình trong file này.

### Cấu hình Biến Mạng
Vì sử dụng Snort như một Host Based IDS, bạn cần đặt giá trị `HOME_NET` là địa chỉ IP của máy Ubuntu. Mở file `/etc/snort/snort.conf` bằng trình chỉnh sửa yêu thích của bạn (ở đây sử dụng VIM):

```bash
sudo vim /etc/snort/snort.conf
```

Sau đó, thay đổi giá trị `HOME_NET` như sau:

```bash
var HOME_NET 192.168.x.x/24  # Thay "192.168.x.x" bằng địa chỉ IP của máy Ubuntu
```

### Kiểm tra Đường dẫn và File Quy tắc
Biến `RULE_PATH` trong file `snort.conf` xác định vị trí của các file quy tắc Snort. Các quy tắc của bạn sẽ được lưu trong file `/etc/snort/rules/local.rules`.

Snort cài đặt kèm theo một số quy tắc cộng đồng mặc định. Ví dụ: Để kiểm tra quy tắc liên quan đến backdoor, bạn có thể xem file `backdoor.rules`.

Để thử nghiệm Snort hiệu quả, hãy tạm thời vô hiệu hóa các quy tắc cộng đồng và chỉ sử dụng file `local.rules` chứa quy tắc của bạn.

### Cấu hình File Output
Để tạo file log phân tích cảnh báo theo mẫu lưu lượng, có thể sử dụng các phương pháp sau. Trong bài này, log sẽ được ghi dưới dạng file CSV và PCAP.

- Để ghi log dạng CSV, thêm dòng sau vào phần "Configure output plugins" trong file `snort.conf`:

```bash
output alert_csv: /var/log/snort/alert.csv default
```

- Để ghi log dạng PCAP, thêm dòng sau vào cùng phần:

```bash
output log_tcpdump: /var/log/snort/tcpdump.log
```

### Kiểm tra File Cấu hình
Kiểm tra file cấu hình với lệnh sau. Nếu có lỗi trong file cấu hình, lệnh này sẽ thông báo lỗi.

```bash
sudo snort -T -i ens33 -c /etc/snort/snort.conf
```

Nếu mọi thứ được cấu hình đúng, bạn sẽ thấy thông báo "Snort successfully validated the configuration!" ở cuối màn hình.

# Phát hiện tấn công SSH BruteForce
https://drive.google.com/file/d/1mxWrY7YiBgDIaUWt_oxvybwRFGCQ-dMU/view?usp=sharing
