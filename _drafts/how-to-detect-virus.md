
Tìm kiếm parent process của một process có pid = 1111
	ps -p 1111 -o ppid=

Theo dõi băng thông mạng tổng thể(live)
	vnstat -i enp7s0 -l

Theo dõi băng thông mạng theo từng kết nối
	iftop
Sau đó nhấn N, S, D để hiển thị theo số, source port, destination port

Tìm process có kết nối ở cổng xxx
	lsof -i :xxx

Tìm đường dẫn cwd của 1 process pid = xxx
	ll /proc/xxx/cwd

Cập nhật lại phần mềm khi dính virus/trojan
ví dụ troạn vào /bin/ps, ta cần cài lại ps, thử dùng yum remove procps
yum remove procps-ng
nhưng có dependencies, như vậy cần xóa phần mềm mà ko xóa dep, dùng lệnh sau:
rpm -e --nodeps procps-ng
sau đó install lại
yum install procps-ng

Tìm các file thay đổi trong 2 ngày gần đây(2 là 2 ngày, -2 là 2 ngày đổ lại, +2 là trước 2 ngày, mtime là modify time, atime là access time, ctime là change time)
find /usr /etc/ /bin/ -mtime -2

stat /path/to/file

Tìm các phần mềm và các cổng mở cho backdoor
đầu tiên liệt kê các cổng mở và xem xét xem có cổng nào là lạ không
[root@5play ~]# ss -an | grep -v 127.0.0.1

sau đó dựa trên cổng lạ trên máy, ví dụ 44554, tìm process mở cổng
[root@5play ~]# lsof -i :44554
COMMAND   PID USER   FD   TYPE     DEVICE SIZE/OFF NODE NAME
Fws     30345 root    3u  IPv4 1760506849      0t0  TCP sourcesatisfy.com:44554->222.186.15.97:10991 (ESTABLISHED

dựa trên process 30345, tìm file chạy
[root@5play ~]# ll /proc/30345/exe
lrwxrwxrwx. 1 root root 0 Mar 24 08:20 /proc/30345/exe -> /tmp/Fws (deleted)

(ở đây file Fws đã phát hiện ra trước và xóa quyền exec nên nó báo deleted)

tiếp theo kill process 30345 đi
[root@5play trojan]# kill -9 30345

chú ý, các phần mềm lsof, ps hoàn toàn có thể bị thay thế với mục đích làm trong suốt trojan với chúng ta, vì thế cần phải cập nhật lại các phần mềm này

nếu chattr bị sửa đổi như sau
[root@5play tmp]# ll /usr/bin/chattr
-rw-r--r--. 1 root root 11528 Jun 10  2014 /usr/bin/chattr
[root@5play tmp]# lsattr /usr/bin/chattr
----i--------e-- /usr/bin/chattr
thì ta không thể xóa các file bị chattr +i, vì chattr không có quyền chạy, và ta cũng không sửa được chattr, giải pháp là(copy file chattr mới, dùng file đó để sửa lỗi):
	cp chattr /tmp && chmod u+x /tmp/chattr && /tmp/chattr -i /usr/bin/chattr && chmod u+x /usr/bin/chattr && rm -f /tmp/chattr
