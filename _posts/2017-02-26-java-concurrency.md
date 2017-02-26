---
title: "Java Concurrency"
layout: post
description: "Ghi chú của tôi khi tìm hiểu về Java Concurrency, tuy vẫn còn dang dở nhưng bạn có thể xem các tài liệu tham khảo"
categories: java
keywords: java, concurrency, multithreading
---
# Sơ lược về Thread

Thread được triền khai thành 2 loại: User Level Thread(Green Thread) và Kernel Level Thread(Native Thread). User Level Thread không thể nhận biết được nó đang chạy ở trên 1 hay nhiều lõi, trong khi Kernel Level Thread có thể tối ưu được với vi xử lý nhiều lõi



Thread được lập lịch để chạy bởi Thread Scheduler. Trong Java, Thread Scheduler lập lịch dựa trên khái niệm Priority: mỗi Thread được gán cho 1 Priority nằm trong khoảng MIN_PRIORITY - MAX_PRIORITY, Thread Scheduler sẽ ưu tiên chạy những Thread nào có Priority cao nhất, nếu các Thread có cùng Priority thì 1 Thread bất kỳ sẽ được chọn.

Thread có các trạng thái New, Runnable, Blocked, Waiting, Timed Waiting, Terminated. Thread lúc mới tạo ra có trạng thái New, khi Thread được chạy có trạng thái Runnable, khi Thread được lập lịch chạy nhưng chưa thể chạy được do Thread Scheduler không lập lịch được(Do có các Thread có Priority cao hơn đang chạy) có trạng thái Blocked. Khi Thread chờ đợi thì tùy theo vô thời hạn hay có thời hạn mà ở trạng thái Waiting hoặc Timed Waiting. Khi Thread chạy xong sẽ có trạng thái Terminated.

Chỉ khi một Thread đang ở trạng thái Runnable chuyển sang trạng thái Blocked/Waiting/Timed Waiting/Terminated, Thread Scheduler mới lập lịch cho các Thread còn lại, còn không Thread Scheduler sẽ không lập lịch dẫn đến vấn đề gọi là Thread Starvation: 1 số Thread không được lập lịch để chạy. 
Thread Starvation đối với các Thread có Priority ngang nhau có thể được giải quyết bằng Time-slicing Scheduler: Scheduler chỉ chạy 1 Thread trong 1 khoảng thời gian cố định, sau đó nó sẽ trao cơ hội cho Thread khác cùng Priority
Thread Starvation đối với các Thread có Priority thấp có thể được giải quyết bằng Priority Boosts: Scheduler sẽ tăng Priority của các Thread có Priority thấp theo thời gian để tăng cơ hội Thread đó được chạy. Khi Thread đó được chạy Priority sẽ được thiết lập về giá trị ban đầu

Một vấn đề khác gọi là Thread Priority Inversion. Có 3 Thread với 3 mức Priority cao - H, trung - M, thấp - L. Ban đầu Thread L giữ 1 Lock, 1 thời gian sau Thread H chạy và chờ Lock đó, tiếp theo Thread M chạy mãi mãi. Bởi vì Thread M chạy mãi mãi dẫn đến Thread L không được chạy, Lock không được giải phóng, dẫn đến Thread H không được chạy. Vấn đề này được giải quyết bằng Priority Inheritance: Thread M sẽ tạm thời có Priority bằng Thread H trong thời gian nó giữ Lock, và vì thế nó có cơ hội để chạy và giải phóng Lock, lúc đó Priority của nó sẽ được thiết lập về giá trị ban đầu.

Thread.yield: Thread đang chạy trao quyền lại cho Thread Scheduler lập lịch cho Thread khác chạy, tuy nhiên Thread khác có thể là chính Thread trao quyền

ThreadLocal: Tạo biến cục bộ cho từng Thread. Một cách sử dụng ThreadLocal là nó được tạo và dùng ở các Class khác không phải Thread hiện tại

Thread.wait

Volatile keyword

Synchronized keyword

# Java concurency library

Sự khác biệt của Semaphore và Mutex là Mutex chỉ giải phóng Lock bởi cùng 1 Thread, còn Semaphore có thể giải phóng Lock bởi 1 Thread bất kỳ

# Java memory model

Sequential consistency memory model > Java memory model > Happen before memory model

Sequential consistency memory model không giải quyết được vấn đề atomic của 1 tập các lệnh

Sự khác biệt giữa Java memory model và Happen before memory model nằm ở chỗ JMM hỗ trợ Causality tránh 1 số vấn đề như "Out of thin air"

Nếu coi các "lệnh" - các Action trong chương trình là các đỉnh của đồ thị, thì mối quan hệ giữa chúng về mặt thời gian là các cạnh của đồ thị

Program order: total order

Happen before order: partial order, trong các Thread là total order, nhưng giữa các Thread thì chỉ là partial order

Synchronization order: total order

Synchronize-with edges => Happen-before edges, Happen-before edges là transitive closure của Synchroniza-with edges trong Program order


# Tài liệu tham khảo

**Các tài liệu về JMM**


* [Oracle - JSL chapter 17 Threads and Locks](https://docs.oracle.com/javase/specs/jls/se8/html/jls-17.html): Đây là tài liệu chính thống về JMM, tuy nhiên khá khó hiểu vì thế tôi phải truy tra các tài liệu khác như JSR 133, JMM FAQ, Out of thin air, v.v để hiểu về nó
* [William Pugh - The Java Memory Model](https://www.cs.umd.edu/~pugh/java/memoryModel/): Tài liệu mang tính lịch sử về JMM, bao gồm JSR 133, JMM FAQ (3 link ngay dưới)
* [William Pugh - JSR 133](https://www.cs.umd.edu/~pugh/java/memoryModel/jsr133.pdf): JMM được xây dựng dựa trên tài liệu này, tuy nhiên JMM đã sắp xếp lại dẫn đến khó hiểu hơn
* [William Pugh - Threads and Locks(may be draft version of JSL)](https://www.cs.umd.edu/~pugh/java/memoryModel/chapter.pdf)
* [William Pugh - Java Memory Model FAQ](https://www.cs.umd.edu/~pugh/java/memoryModel/jsr-133-faq.html)
* [Doug Lea - JSR-133 Cookbook](http://gee.cs.oswego.edu/dl/jmm/cookbook.html)
* [IBM - Fixing the Java Memory Model, Part 1](https://www.ibm.com/developerworks/library/j-jtp02244/): Bài báo mang tính lịch sử nêu nên các yếu điểm của Java trước phiên bản 1.5, dẫn đến sự ra đời của JMM
* [IBM - Fixing the Java Memory Model, Part 2](https://www.ibm.com/developerworks/library/j-jtp03304/): Bài báo bổ trợ cho Part 1, nêu 1 số khía cạnh của JMM
* [Aleksey Shipilёv - Java Memory Model Pragmatics](https://shipilev.net/blog/2014/jmm-pragmatics/): một bài giải thích tương đối dài về JMM, tuy tôi chưa đọc hết nhưng tôi đánh giá cao giá trị của nó và tôi sẽ đọc khi có thời gian

**Vấn đề "Out of thin air"**

* [Jeff Preshing - Memory Ordering at Compile Time, section Out-Of-Thin-Air Stores](http://preshing.com/20120625/memory-ordering-at-compile-time/)
* [Stackoverflow - How to understand JSR-133 Happens-Before is too Weak Figure 6/7](http://stackoverflow.com/questions/34013636/how-to-understand-jsr-133-happens-before-is-too-weak-figure6-7)
* [Hans J. Boehm - Threads Cannot be Implemented as a Library](http://www.hpl.hp.com/techreports/2004/HPL-2004-209.pdf)
* [Hans J. Boehm - N2176 Memory Model Rationales](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2176.html)
* [Hans J. Boehm - Outlawing Ghosts: Avoiding Out-of-Thin-Air Results](http://plrg.eecs.uci.edu/publications/mspc14.pdf)
* [Hans J. Boehm - N2338 Concurrency memory model compiler consequences](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2338.html)
* [Mark Batty, Peter Sewell - The thin air problem](http://www.cl.cam.ac.uk/~pes20/cpp/notes42.html)
* [Jian Cao - Java Memory Model Lecture Notes](https://www.cs.rice.edu/~johnmc/comp522/lecture-notes/COMP522-2016-Lecture10-Java-MemoryModel.pdf)

**Lý thuyết**

* [Wikipedia - Consistency model](https://en.wikipedia.org/wiki/Consistency_model)

**Các bài báo liên quan đến Concurrency**

* [Javaworld - articles about Java Threads](http://www.javaworld.com/article/2074217/java-concurrency/java-101--understanding-java-threads--part-1--introducing-threads-and-runnables.html)
* [Jenkov - Java Concurency Tutorial](http://tutorials.jenkov.com/java-concurrency/index.html)
* [Java docs -  Object wait/notify](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait--)
* [Why does spurious wakeup occur](http://blog.vladimirprus.com/2005/07/spurious-wakeups.html)
* [Mutex vs Semaphore](https://barrgroup.com/Embedded-Systems/How-To/RTOS-Mutex-Semaphore)
* [Java docs - Semaphore](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html)
* [How to stop livelock threads](https://www.codeproject.com/articles/278840/a-livelock-solution)
* [Java deadlock, livelock, starvation examples](https://richardbarabe.wordpress.com/2014/02/21/java-deadlock-livelock-and-lock-starvation-examples/)
* [Happen before visualization](http://www.logicbig.com/tutorials/core-java-tutorial/java-multi-threading/happens-before/)
* [Good explain about program order in Stackoverflow](http://stackoverflow.com/questions/15654276/interpretation-of-program-order-rule-in-java-concurrency)