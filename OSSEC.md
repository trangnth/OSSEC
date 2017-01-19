#OSSEC
##Khái niệm
OSSEC là một hệ thống phát hiện xâm nhập mã nguồn mở

Cho phép khách hàng phát hiện và cảnh báo về những thay đổi trái phép của hệ thống tệp tin và những hành vi độc hại được nhúng trong các file log của các chương trình hoặc ứng dụng 

Cung cấp một máy chủ đơn giản để quản lý các chính sách trên nhiều hệ điều hành.

Thực hiện: 
* Phân tích đăng nhập
* Kiểm tra tính toàn vẹn file:để phát hiện những thay đổi trong hệ thống và cảnh báo khi chúng xảy ra. Nó có thể là một cuộc tấn công hoặc làm dụng bởi 1 cá nhân, hay một lỗi đánh máy của một quản trị viên, bất kỳ tệp tin, thư mục nào tahy đổi sẽ đều được thông báo.
* Giám sát Log: OSSEC thu thập, phân tích và tương quan, liên kết các log và cho bạn biết nếu có đáng ngờ xảy ra (tấn công, sử dụng sai, lỗi,..)
* Phát hiện rootkit: Các hacker muốn dấu đi hành động của họ, nhưng sử dụng phát hện rootkit bạn có thể nhận được thông báo khi hệ thống bị sử đổi theo cách phổ biến.
* Thời gian thực hiện cảnh báo và phản ứng tích cực: cho phép OSSEC phải hành động ngay lập tức khi cảnh báo đặc biệt được kích hoạt.Điều này có thể ngăn chặn một sự cố lây lan trước khi một quản trị viên kịp sử lý.

Hỗ trợ nhiều hệ điều hành khác nhau như Windows, Ubuntu, CentOs,...

Yêu cầu hệ thống có hỗ trợ C, C++ để biện dịch OSEC từ Sources code

##OSSEC Architecture

<img>

https://ossec.github.io/docs/manual/ossec-architecture.html

##Cài đặt OSSEC 
Ta có thể cài đặt từ Source code hay các gói RPM

Địa chỉ download: http://ossec.github.io/

Cài đặt trên Ubuntu 14.04 

###1. Yêu cầu cài đặt:
```
apt-get install build-essential
apt-get install mysql-dev postgresql-dev
```
###2. Cài đặt Manager/Agent
- Tải về phiên bản mới nhất và checksum của nó
```
# wget -U ossec https://bintray.com/artifact/download/ossec/ossec-hids/ossec-hids-2.8.3.tar.gz
# wget -U ossec https://raw.githubusercontent.com/ossec/ossec-docs/master/docs/whatsnew/checksums/2.8.3/ossec-hids-2.8.3.tar.gz.sha256
# cat ossec-hids-2.8.3.tar.gz.sha256
# sha256sum -C  ossec-hids-2.8.3.tar.gz.sha256 ossec-hids-2.8.3.tar.gz
```
- Giải nén và chạy các install.sh
```
 . Tar -zxvf OSSEC-HIDS - tar.gz * ( hoặc gunzip -d ; tar -xvf ) 
  cd OSSEC-hids- *
 ./install.sh
```
Sau đó tùy chọn các cài đặt:
<img1>
<img2>
<img3>
Tiếp tới khi finish:
<img4>

- OSSEC manager lắng nghe trên UDP cổng 1514

- Start OSSEC: 
`# /var/ossec/bin/ossec-control start`

###3. Cấu hình
###manage_agents trên OSSEC server
Chạy `manage_agents`:
`/var/ossec/bin/manage_agents`

<img5>

####Thêm một agent
Chọn **a** nhập tên agent và IP

IP có thể là một địa chỉ cụ thể hoặc 1 dải IP hay cũng có thể để any

<img6>

Nếu là agent đầu tiên được bổ sung vào máy chủ này thì cần khỏi động lại OSSEC `/var/ossec/bin/ossec-control restart`

####Extract the key for an agent

Sau khi thêm một agnet, một key sẽ được tạo ra. Chìa khóa này cần được copy vào agent. Sử dụng tùy chọn `e` nhập ID của agent cần extract

<img7>

####Xóa bỏ một agent

<img8>

Sau khi xóa cần làm mất hiệu lực của key trong `/var/ossec/etc/client.key`

###manage_agent trên OSSEC agents




*Tham khảo:* https://ossec.github.io/docs/
