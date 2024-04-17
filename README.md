# MQTT-BROKER
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


## Bảo mật cho Mosquitto MQTT Broker 

#### Authentication with Username and Password
Có ba lựa chọn để xác thực: tệp mật khẩu, authentication plugins và Unauthenticated acces. Có thể sử dụng kết hợp cả ba lựa chọn.


#### SSL/TLS



Performance testing
````
````
