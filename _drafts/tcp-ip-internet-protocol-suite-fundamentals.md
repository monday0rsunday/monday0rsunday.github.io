---
title: "TCP/IP internet protocol suite fundamentals"
layout: post
description: "Gần như mọi lĩnh vực công nghệ thông tin đều có sự xuất hiện của mạng máy tính, vì thế việc hiểu mạng máy tính là điêu cần thiết đối với một kỹ sư phần mềm. Bài viết sau là một ghi chú riêng nhằm giới thiệu tổng quan về mô hình mạng máy tính đang được dùng ngày nay: bộ giao thức liên mạng TCP/IP và mô hình khái niệm OSI."
categories: network
keywords: Unicast, Multicast, Broadcast, Virtual IP, LAN, WAN, VLAN, VPN, TLS
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

OSI thường dùng làm mô hình tham chiếu khi nhắc tới mạng máy tính. Trong các mục tiếp theo, khi tham chiếu mô hình mạng tôi sẽ sử dụng 4 tầng từ vật lý tới giao vận, thêm 1 tầng ứng dụng của OSI, và tất cả các tầng của mô hình TCP/IP. Tôi cũng dùng từ viết tắt L1,...L7 cho các tầng này, từ tầng vật lý đến tầng ứng dụng.

# Tâng giao tiếp mạng và một số giao thức Ethernet, PPP, X.25, Frame Relay

# Tầng mạng và một số giao thức IP, ARP, ICMP, IGMP

1. Network, IP Address, Subnet, Supernet, Network Mask, CIDR

2. IP Routing

# Tầng vận chuyển và một số giao thức TCP, UDP, SCTP

# Tản mạn về mạng máy tính

1. Khái niệm Hub, Switch, Router, Modem, Gateway

   TL;DR, Với sự phát triển mạnh mẽ của Internet, thông tin trở nên ngập tràn và là một mớ hỗn độn các khái niệm cũng như thuật ngữ. Người viết truyền đạt thông tin theo ý hiểu của họ, thay vì các định nghĩa chuẩn, khiến cho người đọc rất dễ nhầm lẫn. Các khái niệm Hub, Switch, Router, Modem hay Gateway tôi được nghe từ khi bắt đầu dùng máy tính kết nối mạng. Tuy nhiên với tôi chúng là các khái niệm rất mơ hồ cho đến khi tôi thực sự tìm hiểu về mạng máy tính, vì thế tôi cảm thấy cần chia sẻ chúng ở đây.

   Hub: 2 máy tính chỉ giao tiếp với nhau khi chúng được kết nối vật lý với nhau và giao tiếp bằng cách truyền các dữ liệu số trên kết nối vật lý đó. Để cho N máy tính giao tiếp với nhau chúng ta cần N*(N-1) kết nối vật lý, một con số không tưởng. Hub ra đời làm nhiệm vụ trung chuyển ở tầng L1: tất cả N máy tính kết nối vật lý với Hub, khi một máy tính A cần giao tiếp với máy tính B, máy tính A truyền dữ liệu số tới Hub, Hub sẽ truyền dữ liệu số tới tất cả các máy khác ngoài A, trong đó có B, vì thế A giao tiếp được với B.

   Switch: Khi dùng Hub một vấn đề nảy sinh ra đó là băng thông trên các kết nối vật lý với Hub bị lãng phí vô ích vì Hub truyền dữ liệu số với cả các máy không phải đích đến của dữ liệu số. Switch ra đời nhằm mục đích tối ưu lại Hub, chỉ truyền dữ liệu số tới đúng đích đến. Switch làm việc ở tầng L2 giao tiếp bằng các khung dữ liệu, ở tầng này các máy được đánh địa chỉ vật lý. Tại thời điểm ban đầu Switch làm việc giống như Hub, thêm vào đó Switch sẽ phân tích khung dữ liệu xem địa chỉ vật lý ứng với kết nối vật lý là gì và lưu lại thông tin kết nối vật lý-địa chỉ vật lý đó. Khi một máy A truyền khung dữ liệu đến Switch, Switch phân tích xem khung dữ liệu này đến địa chỉ vật lý nào, nếu có thông tin kết nối vật lý-địa chỉ vật lý Switch sẽ chỉ truyền khung dữ liệu tới kết nối vật lý đó, thay vì truyến tất cả như Hub.

   Router: Các địa chỉ vật lý chỉ có ý nghĩa trong phân mạng(Network segment) ứng với tầng L2, đối với liên mạng dùng địa chỉ IP ứng với tầng L3, ta cần một thiết bị truyền các khung dữ liệu tới đích đến - và đó là Router. Router lưu thông tin địa chỉ mạng của các phân mạng/Router kết nối với nó. Khi Router nhận được một gói tin ở tầng L3, nó sẽ xác định xem gói tin này nên truyền tới phân mạng nào thì có khả năng tới đích đến, và đẩy gói tin sang liên kết vật lý ứng với phân mạng/Router đó.

   Bên cạnh Router làm việc ở L3, cũng có Switch làm việc ở L3, về mặt chức năng thì chúng đều đẩy gói tin giống nhau, tuy nhiên Switch "cache" lại việc đẩy gói tin này trong khi Router mỗi lần đẩy gói tin đều phân tích để lựa chọn đường đi tối ưu. Switch L3 xử lý nhanh hơn Router, và Switch L3 hay được triển khai bằng phần cứng(Thông tin này tôi tham khảo trên mạng, bạn đọc vui lòng kiểm chứng lại nếu trích dẫn)

   Modem: Các kết nối vật lý chỉ truyền tín hiệu, vì thế để truyền được dữ liệu số, ta cần một thiết bị chuyển đổi từ dữ liệu số sang tín hiệu và ngược lại, và đó là Modem. Một số Modem ta biết đến phổ biến như Modem cáp quang, Modem ADSL. Khái niệm Modem tôi thấy hay dùng khi chúng ta cần truyền tín hiệu qua lại thông qua nhà cung cấp dịch vụ mạng, nhưng rõ ràng cả Switch, Router đều phải có Modem nhúng ở trong thì mới có khả năng truyền dữ liệu số được :)

   Gateway: Với các thiết bị Hub, Switch, Router tôi nói ở trên, việc truyền tín hiệu của chúng được thực hiện trong một môi trường tương đối đồng nhất, ví dụ đều truyền qua dây nối, hay đều truyền qua cáp quang, v.v. Nhưng mạng máy tính thì không đồng nhất, chúng ta có thể truyền trong phân mạng qua sóng Wifi, nhưng truyền liên mạng qua cáp quang. Vì thế khái niệm Gateway ra đời thể hiện thiết bị dùng để tương tác mở giữa 2 môi trường(có thể đồng nhất).

   Thực tế ngày nay các thiết bị mạng kiêm nhiệm nhiều chức năng, các khái niệm Hub, Switch, Router, Modem, Gateway tôi trình bày ở đây chỉ có ý nghĩa tham chiếu về mặt lịch sử. Việc rạch ròi các khái niệm này tôi cảm thấy sẽ giúp ích cho việc hiểu được cách thức hoạt động của mạng máy tính. Nếu như tôi có gì đó hiểu sai hoặc trình bày chưa rõ, bạn đọc vui lòng góp ý :).

2. Biên dịch địa chỉ mạng - NAT(Network Address Translation)

   Các địa chỉ được nhắc tới trong bài viết này, dù là địa chỉ vật lý, địa chỉ IP, v.v đều có thể giả mạo/thiết lập tùy ý bởi máy tính. Để đảm bảo tính tin cậy, các địa chỉ này được cấp phát và kiểm chứng bởi một bên thứ 3, chẳng hạn như địa chỉ IP của mạng diện rộng cấp phát bởi IANA, địa chỉ IP của mạng cục bộ cấp phát bởi một máy tính cài đặt DHCP, v.v. Trong khi địa chỉ IP mạng cục bộ là tùy ý do chúng ta kiểm soát hoàn toàn thì địa chỉ IP mạng diện rộng bắt buộc phải thông qua IANA. Việc cấp phát mỗi máy tính(trong một phân mạng) một địa chỉ IP mạng diện rộng có thể lãng phí, nhất là với các máy tính cá nhân. Vì thế 1 quá trình biên dịch địa chỉ mạng - NAT ra đời. Tất cả máy tính trong mạng cục bộ N1 giao tiếp với mạng diện rộng qua 1 Gateway G1, để 2 máy tính ở mạng giao tiếp với nhau chúng cần biết địa chỉ IP của nhau, máy tính C1 trong mạng cục bộ N1 biết địa chỉ IP của máy tính C2 ở mạng diện rộng, nhưng máy tính C2 chỉ biết địa chỉ IP của Gateway G1 ở mạng cục bộ N1. Vì thế khi C1 muốn giao tiếp với C2 qua G1, G1 sẽ lưu thông tin truyền đi từ C1 đến C2, và viết lại địa chỉ gói tin từ C1 thành G1 rồi chuyển đến C2, khi C2 phản hồi lại G1, do G1 có thông tin truyền đi từ C1 đến C2, G1 sẽ đẩy gói tin đó về đúng C1.

   Kỹ thuật biên dịch địa chỉ mạng không chỉ áp dụng với địa chỉ IP, mà còn có thể áp dụng với cổng TCP/UDP ứng dụng đang dùng, và được phân chia như sau:

   * Biên dịch địa chỉ nguồn
      * Nếu áp dụng cho địa chỉ IP cố định: SNAT
      * Nếu áp dụng cho địa chỉ IP bất kỳ trên 1 giao tiếp mạng cố định: IP Masquerade
   * Biên dịch địa chỉ đích - DNAT
   * Biên dịch cổng - PAT