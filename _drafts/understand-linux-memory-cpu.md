---
title: "Understand Linux memory & CPU"
layout: post
description: "Some note about linux memory and CPU"
categories: linux
---
Hôm qua server gặp vấn đề với ssh, không thể kết nối được, ban đầu tưởng bị tấn công vào cổng 22, nhưng sáng nay vào được và thấy gặp vấn đề "Cannot allocate memory" khi chạy bất cứ lệnh nào thì mình đoán nguyên nhân do ssh không thể xin cấp phát bộ nhớ được:

	[root@localhost ~]# ps aux | grep java
	-bash: fork: Cannot allocate memory

Kiểm tra lại bộ nhớ bằng `top` thì thấy Mem used lên gần 100%, do đó ban đầu mình tưởng do bộ nhớ đầy nên khởi động lại 1 số chương trình (mỗi cái chiếm 4-8GB RSS theo `ps aux`), nhưng sau chợt nhận thấy chỉ có 1 vài chương trình đó, làm sao ngốn hết được những 50GB Mem , vì thế mình cộng lại tất cả RSS và thấy nó lệch so với Mem used: SUM(RSS) = 16GB trong khi Mem used = 24GB

	[root@localhost webapps]# top
	top - 11:08:20 up 7 days, 20:38,  6 users,  load average: 1,80, 116,59, 101,97
	Tasks: 242 total,   1 running, 241 sleeping,   0 stopped,   0 zombie
	%Cpu(s):  6,2 us,  0,5 sy,  0,0 ni, 92,8 id,  0,5 wa,  0,0 hi,  0,0 si,  0,0 st
	KiB Mem:  49263788 total, 24554872 used, 24708916 free,   303336 buffers
	KiB Swap: 20971516 total,     3432 used, 20968084 free.  7604276 cached Mem

	  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                                             
	32546 root      20   0 10,731g 3,072g  26876 S  12,8  6,5   5:58.01 java                                                                                                
	10496 root      20   0 14,268g 333744  13692 S   6,4  0,7   0:05.42 java                                                                                                
	16849 root      20   0 5605344 484956  12008 S   6,4  1,0  19:04.83 java                                                                                                
	    1 root      20   0  198216   7084   2316 S   0,0  0,0   1:05.85 systemd


	[root@localhost webapps]# ps aux | awk '{sum=sum+$6}END{print sum}'
	15167112

Sau 1 hồi google mình đã có đáp án: [process swap and memory usage](http://serverfault.com/questions/372430/ubuntu-linux-process-swap-memory-and-memory-usage/372594#answer-372594):

	The linux virtual memory system isnt quite so simple. You cant just add up all the RSS fields and get the value reported used by free. There many reasons for this, but I''ll hit a couple of the biggest ones.

	When a process forks, both the parent and the child will show with the same RSS. However linux employs a copy-on-write so that both processes are really using the same memory. Only when one of the processes modifies the memory will it actually be duplicated.
	    So this will cause the free number to be smaller than the top RSS sum.

	The RSS value doesnt include shared memory. Because shared memory isnt owned by any 1 process, top doesnt include it in RSS. So this will cause the free number to be larger than the top RSS sum.

Tìm hiểu chi tiết thêm thì mình có thêm thông tin tương tự thú vị về lệnh `top` vs `free` [Linux ate my RAM](http://www.linuxatemyram.com/). Trong linux, linux thường mượn unused memory cho disk caching nhằm mục đích làm cho hệ thống chạy mượt hơn. Khi đã mượn thì memory sẽ trở thành used, dù có được dùng hay không, còn đối với chúng ta, memory được dùng mới tính là used, do đó có sự hiểu lầm ở đây. Bảng sau làm rõ các khái niệm này:

|Memory that is | You''d call it | Linux calls it|
|taken by applications | Used |	Used|
|available for applications, and used for something |	Free |	Used|
|not used for anything |	Free |	Freev|

Khi chúng ta chạy free, chúng ta sẽ thấy rõ ràng điều này: Mem đã sử dụng là 26GB, free là 23 GB, nhưng đối với buffer, thực tế sử dụng là 18GB, free là 31GB. Mem thực tế của chúng ta chỉ khoảng 18GB

	[root@localhost webapps]# free
		     total       used       free     shared    buffers     cached
	Mem:      49263788   26561976   22701812     803100     303772    7943832
	-/+ buffers/cache:   18314372   30949416
	Swap:     20971516       3432   20968084

Xem thêm ví dụ minh hoạ về việc sử dụng disk caching: [Experiments and fun with the Linux disk cache](http://www.linuxatemyram.com/play.html)
Trong [tài liệu Red Hat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/chap-Oracle_9i_and_10g_Tuning_Guide-Memory_Usage_and_Page_Cache.html) cũng có nói về vấn đề này

	5.1. Checking the Memory Usage
	To determine the size and usage of memory, you can enter the following command:

	grep MemTotal /proc/meminfo

	You can find a detailed description of the entries in /proc/meminfo at http://www.redhat.com/advice/tips/meminfo.html.
	Alternatively, you can use the free(1) command to check the memory:

	$ free
		      total       used        free    shared    buffers    cached
	Mem:        4040360    4012200       28160         0     176628   3571348
	-/+ buffers/cache:      264224     3776136
	Swap:       4200956      12184     4188772
	$

	In this example the total amount of available memory is 4040360 KB. 264224 KB are used by processes and 3776136 KB are free for other applications. Do not get confused by the first line which shows that 28160KB are free! If you look at the usage figures you can see that most of the memory use is for buffers and cache. Linux always tries to use RAM to speed up disk operations by using available memory for buffers (file system metadata) and cache (pages with actual contents of files or block devices). This helps the system to run faster because disk information is already in memory which saves I/O operations. If space is needed by programs or applications like Oracle, then Linux will free up the buffers and cache to yield memory for the applications. If your system runs for a while you will usually see a small number under the field "free" on the first line. 

Tham khảo:

- [Red Hat /proc/meminfo](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s2-proc-meminfo.html)
- [Linux ate my ram!](http://www.linuxatemyram.com/)
- [Experiments and fun with the Linux disk cache](http://www.linuxatemyram.com/play.html)
- [Red Hat - Tuning and Optimizing Red Hat Enterprise Linux for Oracle Database 9i and 10g - ⁠Chapter 5. Memory Usage and Page Cache](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/chap-Oracle_9i_and_10g_Tuning_Guide-Memory_Usage_and_Page_Cache.html)
- [Overview of memory management](http://www.linuxhowtos.org/System/Linux%20Memory%20Management.htm)
- [Red Hat - Tuning and Optimizing Red Hat Enterprise Linux for Oracle Database 9i and 10g](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/part-Oracle_9i_and_10g_Tuning_Guide-Red_Hat_Enterprise_Linux_Oracle_Tuning_Guide-Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_Database_9i_and_10g.html)
- [Red Hat - Deployment Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/index.html)
