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

*Note:* khi cài đặt thì lệnh cài msql và postgresql không chạy thì chạy lệnh sau đây để cài đặt Apache, MySQL, PHP:

```
sudo apt-get install mysql-server libmysqlclient-dev mysql-client apache2 php5 libapache2-mod-php5 php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl
```


###2. Cài đặt Manager/Agent
- Tải về phiên bản mới nhất và checksum của nó
```
# wget -U ossec https://bintray.com/artifact/download/ossec/ossec-hids/ossec-hids-2.8.3.tar.gz
# wget -U ossec https://raw.githubusercontent.com/ossec/ossec-docs/master/docs/whatsnew/checksums/2.8.3/ossec-hids-2.8.3.tar.gz.sha256
# cat ossec-hids-2.8.3.tar.gz.sha256
# sha256sum -c  ossec-hids-2.8.3.tar.gz.sha256 ossec-hids-2.8.3.tar.gz
```
- Giải nén và chạy các install.sh
```
 tar -zxvf ossec-hids-*.tar.gz ( hoặc gunzip -d ; tar -xvf ) 
 cd ossec-hids-*
 ./install.sh
```
Sau đó tùy chọn các cài đặt:

<img1>

<img2>

Nếu là agent

<img2-1>

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
Chạy `/var/ossec/bin/manage_agents`

Chọn `i` để import key từ server vào. Copy key của agent đã được tạo trên server 

<img9>

####mysql trên server
Tạo một Mysql user và database cho ossec: 
```
#mysql -u root -p
mysql> create database ossec;
Query OK, 1 row affected (0.00 sec)

mysql> grant all privileges on ossec.* to ossecuser@localhost identified by 'your_password';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
```
<img11>
 
Tiếp theo chạy lệnh sau và nhập password(trong thư mục đã tải về): 

`mysql -u root -p ossec < src/os_dbd/mysql.schema`

<img12>

Thêm các dòng sau vào tệp tin cấu hình `nano /var/ossec/etc/ossec.conf`
```
<database_output>
        <hostname>127.0.0.1</hostname>
        <username>ossecuser</username>
        <password>your_password</password>
        <database>ossec</database>
        <type>mysql</type>
</database_output>
```
Save and enable database and restart ossec:
```
# /var/ossec/bin/ossec-control enable database
# /var/ossec/bin/ossec-control restart
```

###4.Cài đặt OSSEC WEB UI
```
# cd /var/www/html/
# wget https://github.com/ossec/ossec-wui/archive/master.zip
# unzip master.zip
```
Đổi tên thư mục ossec:
`#mv ossec-wui-master/ ossec/`

Tạo một thư mục **tmp** và thiết lập các quyền sở hữu tệp tin và điểu khiển: 
```
# mkdir ossec/tmp/
# chown www-data: -R ossec/
# chmod 666 /var/www/html/ossec/tmp
```

Bây giờ có thể vào theo địa chỉ: `http://your_server_IP/ossec/`



*Tham khảo:* https://ossec.github.io/docs/

*Note: Tất cả user là `huyentrang`, pass là `123456`*
