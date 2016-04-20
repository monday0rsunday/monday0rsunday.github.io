---
title: "Hướng dẫn thiết lập nagios trong CentOS"
layout: post
---
# Cài đặt nagios
    yum install nagios nagios-plugins-all

Thư mục cấu hình: /etc/nagios
Thư mục plugins: /usr/lib64/nagios/plugins

# Cài đặt NRPE cho việc theo dõi từ xa
1. Máy cần theo dõi(client)

  1.1 Cài đặt

    yum install nagios-nrpe nagios-plugins-all

Thư mục cấu hình: /etc/nagios
Thư mục plugins: /usr/lib64/nagios/plugins

  1.2 Cấu hình

    vi /etc/nagios/nrpe.cfg
  Mở cổng cho phép kết nối tới nrpe

  1.3 Chạy
    
    service nrpe start # hoặc systemctl start nrpe

2. Máy quản lý(server)
    yum install nagios-nrpe-server

# Tham khảo

1. [](http://xmodulo.com/nagios-remote-plugin-executor-nrpe-linux.html)
2. [](https://www.digitalocean.com/community/tutorials/how-to-install-nagios-on-centos-6)
