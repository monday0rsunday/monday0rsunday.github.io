---
layout: post
title: "OSGi ecosystem with Felix, Karaf, Equinox"
description: "Tản mạn về OSGi"
category: linux
author: "congnh"
---

Việc module hóa ứng dụng đã từng là một trào lưu. Với Java, bản thân ngôn ngữ không hỗ trợ module vì thế việc viết ứng dụng với khả năng module hóa trên Java khó khăn, cho đến khi có sự xuất hiện của OSGi.

OSGi container: khi chúng ta viết một ứng dụng OSGi, chúng ta có thể nhúng OSGi core trực tiếp trong ứng dụng. Tuy nhiên điều này khiến cho bản thân ứng dụng trở nên nặng nề, và với quan điểm 'Separation of Concern', OSGi container ra đời nhằm mục đích tập trung vào việc quản lý ứng dụng, còn chúng ta chỉ cần tập trung vào logic của ứng dụng.

Apache Felix, Eclipse Equinox là OSGi core framework, trong khi Karaf là OSGi container.

Karaf có thể sử dụng Felix hoặc Equinox làm OSGi runtime

Spring DM: Spring là IoC framework, nó không hỗ trợ OSGi, vì thế SpringDM ra đời. chúng ta có thể viết 1 ứng dụng OSGi với tính năng IoC sử dụng Spring DM. Sau đó OSGi Blueprint ra đời dựa trên Spring DM nhằm hỗ trợ IoC. Spring DM có thể coi là 1 implementation của Blueprint

Blueprint: Apache Aries, Eclipse Gemini blueprint

[
OSGi: What are the differences between Apache Felix and Apache Karaf?
](http://stackoverflow.com/questions/1612120/osgi-what-are-the-differences-between-apache-felix-and-apache-karaf?rq=1)