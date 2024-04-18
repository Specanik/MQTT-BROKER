# Cái gì mà nhà môi giới muỗi MQTT :)))
## Cài đặt Mosquitto MQTT Broker trên linux
Thêm gói [mosquitto-dev ](https://launchpad.net/~mosquitto-dev/+archive/ubuntu/mosquitto-ppa):

````
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
````
Cập nhật package index:
````
sudo apt-get update
````
Cài đặt Mosquitto:
````
sudo apt-get install mosquitto
````
Cài đặt Mosquitto-client:
````
sudo apt-get install mosquitto-clients
````
Kiểm tra trạng thái của Mosquitto:
````
systemctl status mosquitto.service 
````
Mosquitto chạy mặc định trên cổng 1883, với config mặc định: /etc/mosquitto/mosquitto.conf

Để dừng có thể sử dụng lệnh:
````
systemctl stop  mosquitto.service
````
Hoặc:
````
sudo kill PID
````
Kiểm tra PID bằng lệnh:
````
ps -ef | grep mosquitto

````

## Khởi chạy Mosquitto broker
Khi sử dụng lệnh `mosquitto` thì sẽ sử dụng file cấu hình mặc định `/etc/mosquitto/mosquitto.conf `

Mẫu lệnh:
`mosquitto [-c config file] [ -d | --daemon ] [-p port number] [-v]`


## Viết file cấu hình
Tham khảo tại [đây.](https://mosquitto.org/man/mosquitto-conf-5.html)


 
## Bảo mật cho Mosquitto MQTT Broker 
Có ba lựa chọn sẵn có của Mosquitto để xác thực: sử dụng tệp mật khẩu, authentication plugins và Unauthenticated acces. Có thể sử dụng kết hợp cả ba lựa chọn. Tham khảo tại [đây](https://mosquitto.org/documentation/authentication-methods/)

Ngoài ra dữ liệu cần được mã hóa trên kênh truyền, tại đây ta có thể sử dụng [Openssl](https://www.openssl.org/) hoặc [Letsencrypt](https://letsencrypt.org/) để tạo chứng chỉ SSL miễn phí.
#### Authentication with Username and Password
Đầu tiên ta cần tạo một password file. Ta sử dụng công cụ [```mosquitto_passwd```](https://mosquitto.org/man/mosquitto_passwd-1.html)

```
mosquitto_passwd [ -H hash ] [ -c | -D ] passwordfile username

mosquitto_passwd [ -H hash ] -b passwordfile username password

mosquitto_passwd -U passwordfile
```
***OPTION:***

`-b`
Chạy ở chế độ hàng loạt. Điều này cho phép cung cấp mật khẩu tại dòng lệnh, thuận tiện nhưng nên sử dụng cẩn thận vì mật khẩu sẽ hiển thị trên dòng lệnh và trong lịch sử lệnh.

`-c`
Tạo một tập tin mật khẩu mới. Nếu file đã tồn tại thì nó sẽ bị ghi đè.

`-D`
Xóa người dùng được chỉ định khỏi tệp mật khẩu.

`-H`
Chọn hàm băm để sử dụng, `sha512-pbkdf2` hoặc `sha512`. Mặc định là `sha512-pbkdf2`.

`-U`
Tùy chọn này có thể được sử dụng để nâng cấp/chuyển đổi tệp mật khẩu có mật khẩu văn bản thuần túy thành mật khẩu băm bằng mật khẩu băm. Nó sẽ sửa đổi tập tin được chỉ định. Nó không phát hiện xem mật khẩu đã được băm hay chưa, do đó, việc sử dụng nó trên tệp mật khẩu đã chứa mật khẩu được băm sẽ tạo ra các hàm băm mới dựa trên các hàm băm cũ và khiến tệp mật khẩu không thể sử dụng được.

`passwordfile`
Tệp mật khẩu cần sửa đổi.

`username`
Tên người dùng để thêm/cập nhật/xóa.

`password`
Mật khẩu để sử dụng khi ở chế độ hàng loạt.

***VÍ DỤ:***

Tạo file mật khẩu:
```
mosquitto_passwd -c passwordfile user
```

Cập nhật người dùng mới vào file mật khẩu đó:
```
mosquitto_passwd <passwordfile> <user>
```
*Mật khẩu sẽ được yêu cầu nhập sau khi chạy lệnh.*

**Về cơ bản nếu chỉ sử dụng Username và Password thì file config có thể được viết đơn giản như sau:**
````
listener 1883
allow_anonymous false
password_file file path
````


#### Authentication plugins: [Dynamic Security Plugin](https://mosquitto.org/documentation/dynamic-security/)
#### SSL/TLS
Ở đây chúng ta sẽ sử dụng [Openssl](https://www.openssl.org/)

***Cài Openssl:***
````
sudo apt-get install openssl
````
````
openssl genrsa -des3 -out ca.key 2048
openssl req -new -x509 -days 2000 -key ca.key -out ca.crt
openssl genrsa -out server.key 2048
openssl req -new -out server.csr -key server.key
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 410

````

Performance testing
````
````
