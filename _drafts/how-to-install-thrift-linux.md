---
layout: post
title: "How to install Apache Thrift on Linux machines"
description: "Guide to install Apache Thrift on some Linux machines"
category: linux
author: "congnh"
---

### Ubuntu

1. Install dependencies:

Following guide

    $ sudo apt-get install g++ libboost-all-dev libssl-dev byacc bison flex ant
    $ wget 'http://mirrors.viethosting.vn/apache//ant/binaries/apache-ant-1.9.4-bin.tar.gz'
    $ tar -zxvf apache-ant-1.9.4-bin.tar.gz
    $ sudo bash -c "echo 'export ANT_HOME=/path/to/apache-ant-1.9.4-bin' >> /etc/bash.bashrc"
    $ sudo bash -c "echo 'export PATH=$PATH:$ANT_HOME/bin' >> /etc/bash.bashrc"
    $ source /etc/bash.bashrc

2. Make

Following guide

    $ cd /path/to/thrift
    $ sudo ./boostrap.sh
    $ sudo ./configure --with-boost=/usr/include/boost # BOOST_ROOT dir, which contains file version.hpp
    $ sudo make
    $ sudo make install

