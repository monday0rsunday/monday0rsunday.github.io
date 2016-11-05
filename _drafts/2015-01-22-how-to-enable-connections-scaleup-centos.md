---
layout: post
categories: linux
tag: linux
title: "How to enable connections scaleup in CentOS"
description: "Good softwares must have good platform: You have a 10k concurrent connections webservice, you must have a 10k concurrent connections hardware & operating system. In this post I brief some configurations need for enabling connections scaleup in CentOS."
---

Centos có 1 số tham số giới hạn tài nguyên sử dụng:

    $ ulimit -a
    core file size          (blocks, -c) 0
	data seg size           (kbytes, -d) unlimited
	scheduling priority             (-e) 0
	file size               (blocks, -f) unlimited
	pending signals                 (-i) 30320
	max locked memory       (kbytes, -l) 64
	max memory size         (kbytes, -m) unlimited
	open files                      (-n) 1024
	pipe size            (512 bytes, -p) 8
	POSIX message queues     (bytes, -q) 819200
	real-time priority              (-r) 0
	stack size              (kbytes, -s) 8192
	cpu time               (seconds, -t) unlimited
	max user processes              (-u) 30320
	virtual memory          (kbytes, -v) unlimited
	file locks                      (-x) unlimited

    $ sysctl -a | grep file
	fs.file-nr = 7872	0	16423512
	fs.file-max = 16423512

