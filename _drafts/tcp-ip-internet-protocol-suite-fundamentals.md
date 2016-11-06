---
title: "TCP/IP internet protocol suite fundamentals"
layout: post
description: ""
---

# Giới thiệu về bộ giao thức liên mạng TCP/IP và mô hình OSI

Bộ giao thức liên mạng TCP/IP là một mô hình mạng máy tính và một tập các giao thức truyền thông được sử dụng trên internet và các mạng máy tính tương tự. TCP/IP cung cấp việc truyền thông dữ liệu giữa 2 máy bất kỳ bằng cách quy định làm thế nào dữ liệu được đóng gói, đánh địa chỉ, truyền đi, định tuyến và nhận được. TCP/IP được chia làm 4 tầng trừu tượng dùng để sắp xếp các giao thức liên quan:

* Tầng giao tiếp mạng (Network interface layer): Cung cấp cách thức truyền dữ liệu giữa 2 máy có giao tiếp với nhau trong một mạng cục bộ. Giao tiếp này có thể là giao tiếp vật lý, hoặc giao tiếp ảo
* Tầng liên mạng (Internet layer): Cung cấp cách thức truyền dữ liệu giữa 2 máy nằm trên 2 mạng khác nhau
* Tầng giao vận (Transport layer): Thiết lập kênh truyền dữ liệu giữa 2 máy bất kỳ
* Tầng ứng dụng(Application layer): Thiết lập kênh truyền dữ liệu theo định dạng được sử dụng bởi ứng dụng

Mô hình OSI là một mô hình khái niệm đặc tính hóa và chuẩn hóa các chức năng truyền thông của một hệ thống viễn thông hoặc máy tính không phụ thuộc vào cấu trúc nội tại và công nghệ của chúng. Mô hình OSI phân chia một hệ thống truyền thông thành các tầng trừu tượng như TCP/IP, bao gồm 7 tầng:

* Tầng vật lý (Physical layer): cung cấp cách thức truyền dữ liệu giữa 2 máy qua một đường truyền vật lý
* Tầng liên kết dữ liệu (Data link layer): cung cấp cách thức truyền dữ liệu tin cậy giữa 2 máy kết nối bởi tầng vật lý
* Tầng mạng (Network layer): cung cấp cách thức truyền dữ liệu trong một mạng nhiều máy
* Tầng giao vận (Transport layer): thiết lập truyền dữ liệu tin cậy giữa 2 máy bất kỳ
* Tầng phiên(Session layer): quản lý phiên truyền thông
* Tầng trình diễn (Presentation layer): chuyển đổi dữ liệu giữa một dịch vụ mạng và ứng dụng như mã hóa ký tự, nén dữ liệu, v.v
* Tầng ứng dụng (Application layer):

Trong khi TCP/IP chỉ giải quyết bài toán truyền thông trong một hệ thống máy tính thì OSI ra đời nhằm mục đích giải quyết bài toán tương tác mở giữa hệ thống máy tính và viễn thông, tuy nhiên việc triển khai OSI đã không thành công, vì thế OSI là mô hình lý thuyết chưa được triển khai, còn TCP/IP trở thành mô hình của mạng máy tính ta đang dùng ngày này(Tham khảo [OSI: The Internet That Wasn’t](http://spectrum.ieee.org/computing/networks/osi-the-internet-that-wasnt)) . Ở đây ta có thể coi TCP/IP là hiện thực hóa của OSI, trong đó một số tầng liên tiếp trong OSI được gộp lại thành 1 tầng trong TCP/IP:

| OSI                   | TCP/IP              |
| --------------------- | ------------------- |
| Tầng vật lý           | Tầng giao tiếp mạng |
| Tầng liên kết dữ liệu | Tầng giao tiếp mạng |
| Tầng mạng             | Tầng liên mạng      |
| Tầng giao vận         | Tầng giao vận       |
| Tầng phiên            | Tầng ứng dụng       |
| Tầng trình diễn       | Tầng ứng dụng       |
| Tầng ứng dụng         | Tầng ứng dụng       |

OSI thường dùng làm mô hình tham chiếu khi nhắc tới mạng máy tính. Trong các mục tiếp theo, khi tham chiếu mô hình mạng tôi sẽ sử dụng 4 tầng từ vật lý tới giao vận của OSI, và tầng ứng dụng của mô hình TCP/IP

# Tâng giao tiếp mạng và một số giao thức Ethernet, PPP, X.25, Frame Relay

# Tầng mạng và một số giao thức IP, ARP, ICMP, IGMP

## Network, IP Address, Subnet, Supernet, Network Mask, CIDR

## IP Routing

# Giới thiệu về giao thức TCP, UDP

## Giới thiệu về NAT(SNAT, DNAT, PAT, IP Masquerade)

# Sơ lược về các thiết bị mạng Hub, Switch, Router, Modem, Gateway

# Tổng quan về Network Load Balancing: L2(Network Bonding), L3(NAT, DSR, IP Tunnel), L4, L7

Unicast, Multicast, Broadcast
Virtual IP
LAN, WAN, VLAN, VPN
TLS
IPVS

# ifconfig, iproute, iproute2, netfilter iptable & ebtable