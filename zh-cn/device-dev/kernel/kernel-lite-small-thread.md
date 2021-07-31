# 线程<a name="ZH-CN_TOPIC_0000001051452247"></a>

-   [基本概念](#section1179311337405)
-   [使用场景](#section44877547404)
-   [接口说明](#section2069477134115)

## 基本概念<a name="section1179311337405"></a>

从系统的角度看，线程是竞争系统资源的最小运行单元。线程可以使用或等待CPU、使用内存空间等系统资源，并独立于其它线程运行。

OpenHarmony内核每个进程内的线程独立运行、独立调度，当前进程内线程的调度不受其它进程内线程的影响。

OpenHarmony内核中的线程采用抢占式调度机制，同时支持时间片轮转调度和FIFO调度方式。

OpenHarmony内核的线程一共有32个优先级\(0-31\)，最高优先级为0，最低优先级为31。

当前进程内高优先级的线程可抢占当前进程内低优先级线程，当前进程内低优先级线程必须在当前进程内高优先级线程阻塞或结束后才能得到调度。

**线程状态说明：**

-   初始化（Init）：该线程正在被创建。

-   就绪（Ready）：该线程在就绪列表中，等待CPU调度。

-   运行（Running）：该线程正在运行。

-   阻塞（Blocked）：该线程被阻塞挂起。Blocked状态包括：pending\(因为锁、事件、信号量等阻塞\)、suspended（主动pend）、delay\(延时阻塞\)、pendtime\(因为锁、事件、信号量时间等超时等待\)。

-   退出（Exit）：该线程运行结束，等待父线程回收其控制块资源。


**图 1**  线程状态迁移示意图<a name="fig15746193414436"></a>  
![](figure/线程状态迁移示意图.png "线程状态迁移示意图")

**线程状态迁移说明：**

-   Init→Ready：

    线程创建拿到控制块后为Init状态，处于线程初始化阶段，当线程初始化完成将线程插入调度队列，此时线程进入就绪状态。

-   Ready→Running：

    线程创建后进入就绪态，发生线程切换时，就绪列表中最高优先级的线程被执行，从而进入运行态，但此刻该线程会从就绪列表中删除。

-   Running→Blocked：

    正在运行的线程发生阻塞（挂起、延时、读信号量等）时，线程状态由运行态变成阻塞态，然后发生线程切换，运行就绪列表中剩余最高优先级线程。

-   Blocked→Ready ：

    阻塞的线程被恢复后（线程恢复、延时时间超时、读信号量超时或读到信号量等），此时被恢复的线程会被加入就绪列表，从而由阻塞态变成就绪态。

-   Ready→Blocked：

    线程也有可能在就绪态时被阻塞（挂起），此时线程状态会由就绪态转变为阻塞态，该线程从就绪列表中删除，不会参与线程调度，直到该线程被恢复。

-   Running→Ready：

    有更高优先级线程创建或者恢复后，会发生线程调度，此刻就绪列表中最高优先级线程变为运行态，那么原先运行的线程由运行态变为就绪态，并加入就绪列表中。

-   Running→Exit：

    运行中的线程运行结束，线程状态由运行态变为退出态。若未设置分离属性（PTHREAD\_CREATE\_DETACHED）的线程，运行结束后对外呈现的是Exit状态，即退出态。


## 使用场景<a name="section44877547404"></a>

线程创建后，用户态可以执行线程调度、挂起、恢复、延时等操作，同时也可以设置线程优先级和调度策略，获取线程优先级和调度策略。

## 接口说明<a name="section2069477134115"></a>

OpenHarmony内核系统中的线程管理模块，线程间通信为用户提供下面几种功能：

**表 1**  线程管理模块功能

<a name="table134475244618"></a>
<table><thead align="left"><tr id="row16880122144619"><th class="cellrowborder" valign="top" width="14.299999999999999%" id="mcps1.2.5.1.1"><p id="p88808214619"><a name="p88808214619"></a><a name="p88808214619"></a>头文件</p>
</th>
<th class="cellrowborder" valign="top" width="28.599999999999998%" id="mcps1.2.5.1.2"><p id="p16880182174611"><a name="p16880182174611"></a><a name="p16880182174611"></a>名称</p>
</th>
<th class="cellrowborder" valign="top" width="22.38%" id="mcps1.2.5.1.3"><p id="p198806214469"><a name="p198806214469"></a><a name="p198806214469"></a>说明</p>
</th>
<th class="cellrowborder" valign="top" width="34.72%" id="mcps1.2.5.1.4"><p id="p1188013294618"><a name="p1188013294618"></a><a name="p1188013294618"></a>备注</p>
</th>
</tr>
</thead>
<tbody><tr id="row1188092104619"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p488052134616"><a name="p488052134616"></a><a name="p488052134616"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p108808218461"><a name="p108808218461"></a><a name="p108808218461"></a>pthread_attr_destroy</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p18809234612"><a name="p18809234612"></a><a name="p18809234612"></a>销毁线程属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1988012213468"><a name="p1988012213468"></a><a name="p1988012213468"></a>-</p>
</td>
</tr>
<tr id="row28802274616"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p148806212469"><a name="p148806212469"></a><a name="p148806212469"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p7880720461"><a name="p7880720461"></a><a name="p7880720461"></a><span>pthread_attr_getinheritsched</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p588012104619"><a name="p588012104619"></a><a name="p588012104619"></a>获取线程属性对象的调度属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p11880524466"><a name="p11880524466"></a><a name="p11880524466"></a>-</p>
</td>
</tr>
<tr id="row1888132164611"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p8881202114618"><a name="p8881202114618"></a><a name="p8881202114618"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p13881142154614"><a name="p13881142154614"></a><a name="p13881142154614"></a><span>pthread_attr_getschedparam</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p12881142144612"><a name="p12881142144612"></a><a name="p12881142144612"></a>获取线程属性对象的调度参数属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p988112204611"><a name="p988112204611"></a><a name="p988112204611"></a>-</p>
</td>
</tr>
<tr id="row788102134613"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p16881122114611"><a name="p16881122114611"></a><a name="p16881122114611"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p388111215466"><a name="p388111215466"></a><a name="p388111215466"></a><span>pthread_attr_getschedpolicy</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p19881152174612"><a name="p19881152174612"></a><a name="p19881152174612"></a>获取线程属性对象的调度策略属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p988112294619"><a name="p988112294619"></a><a name="p988112294619"></a><span id="text174691111144919"><a name="text174691111144919"></a><a name="text174691111144919"></a>OpenHarmony</span>：支持SCHED_FIFO 、SCHED_RR调度策略。</p>
</td>
</tr>
<tr id="row14881423468"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p588152114611"><a name="p588152114611"></a><a name="p588152114611"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p188811626465"><a name="p188811626465"></a><a name="p188811626465"></a><span>pthread_attr_getstacksize</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p08825214467"><a name="p08825214467"></a><a name="p08825214467"></a>获取线程属性对象的堆栈大小。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p8882827467"><a name="p8882827467"></a><a name="p8882827467"></a>-</p>
</td>
</tr>
<tr id="row088212144619"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p688215274612"><a name="p688215274612"></a><a name="p688215274612"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p88827218466"><a name="p88827218466"></a><a name="p88827218466"></a><span>pthread_attr_init</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p14447368158"><a name="p14447368158"></a><a name="p14447368158"></a>初始化线程属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p12882202134618"><a name="p12882202134618"></a><a name="p12882202134618"></a>-</p>
</td>
</tr>
<tr id="row1788214210462"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p3882525463"><a name="p3882525463"></a><a name="p3882525463"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1882528461"><a name="p1882528461"></a><a name="p1882528461"></a><span>pthread_attr_setdetachstate</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p11455363157"><a name="p11455363157"></a><a name="p11455363157"></a>设置线程属性对象的分离状态。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1788202174613"><a name="p1788202174613"></a><a name="p1788202174613"></a>-</p>
</td>
</tr>
<tr id="row188829211469"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1088222134619"><a name="p1088222134619"></a><a name="p1088222134619"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p588272204612"><a name="p588272204612"></a><a name="p588272204612"></a><span>pthread_attr_setinheritsched</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p14611364154"><a name="p14611364154"></a><a name="p14611364154"></a>设置线程属性对象的继承调度属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p3883102174611"><a name="p3883102174611"></a><a name="p3883102174611"></a>-</p>
</td>
</tr>
<tr id="row1588310244610"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p118831210461"><a name="p118831210461"></a><a name="p118831210461"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p188318214611"><a name="p188318214611"></a><a name="p188318214611"></a><span>pthread_attr_setschedparam</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1746636141515"><a name="p1746636141515"></a><a name="p1746636141515"></a>设置线程属性对象的调度参数属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p44251243115214"><a name="p44251243115214"></a><a name="p44251243115214"></a><span id="text1222517176498"><a name="text1222517176498"></a><a name="text1222517176498"></a>OpenHarmony</span>：设置线程优先级的参数值越小，线程在系统中的优先级越高；设置参数值越大，优先级越低。</p>
<p id="p142941635183917"><a name="p142941635183917"></a><a name="p142941635183917"></a>注意：需要将pthread_attr_t线程属性的inheritsched字段设置为PTHREAD_EXPLICIT_SCHED，否则设置的线程调度优先级将不会生效，系统默认设置为PTHREAD_INHERIT_SCHED。</p>
</td>
</tr>
<tr id="row118831264610"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1688315220469"><a name="p1688315220469"></a><a name="p1688315220469"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1488320217468"><a name="p1488320217468"></a><a name="p1488320217468"></a><span>pthread_attr_setschedpolicy</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p154615365156"><a name="p154615365156"></a><a name="p154615365156"></a>设置线程属性对象的调度策略属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p28831823468"><a name="p28831823468"></a><a name="p28831823468"></a><span id="text2826162394913"><a name="text2826162394913"></a><a name="text2826162394913"></a>OpenHarmony</span>：支持SCHED_FIFO 、SCHED_RR调度策略。</p>
</td>
</tr>
<tr id="row888320244618"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p88840216460"><a name="p88840216460"></a><a name="p88840216460"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p3884328461"><a name="p3884328461"></a><a name="p3884328461"></a><span>pthread_attr_setstacksize</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p158841329461"><a name="p158841329461"></a><a name="p158841329461"></a>设置线程属性对象的堆栈大小。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1488412244619"><a name="p1488412244619"></a><a name="p1488412244619"></a>-</p>
</td>
</tr>
<tr id="row168841629468"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p988452104612"><a name="p988452104612"></a><a name="p988452104612"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p19884162194610"><a name="p19884162194610"></a><a name="p19884162194610"></a>pthread_getattr_np</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p64812367153"><a name="p64812367153"></a><a name="p64812367153"></a>获取已创建线程的属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p108847254615"><a name="p108847254615"></a><a name="p108847254615"></a>-</p>
</td>
</tr>
<tr id="row28842029469"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1788412214613"><a name="p1788412214613"></a><a name="p1788412214613"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p188411220465"><a name="p188411220465"></a><a name="p188411220465"></a><span>pthread_cancel</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p08001317121519"><a name="p08001317121519"></a><a name="p08001317121519"></a>向线程发送取消请求。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1288416254611"><a name="p1288416254611"></a><a name="p1288416254611"></a>-</p>
</td>
</tr>
<tr id="row788418214464"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p128844204618"><a name="p128844204618"></a><a name="p128844204618"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p17885182194616"><a name="p17885182194616"></a><a name="p17885182194616"></a>pthread_testcancel</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1279931731513"><a name="p1279931731513"></a><a name="p1279931731513"></a>请求交付任何未决的取请求。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1588552204613"><a name="p1588552204613"></a><a name="p1588552204613"></a>-</p>
</td>
</tr>
<tr id="row98857211461"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p5885102174614"><a name="p5885102174614"></a><a name="p5885102174614"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1388582144612"><a name="p1388582144612"></a><a name="p1388582144612"></a>pthread_setcanceltype</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p2079991771517"><a name="p2079991771517"></a><a name="p2079991771517"></a>设置线程可取消类型。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p138854224619"><a name="p138854224619"></a><a name="p138854224619"></a>-</p>
</td>
</tr>
<tr id="row1988516211466"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p68851929468"><a name="p68851929468"></a><a name="p68851929468"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p288515234613"><a name="p288515234613"></a><a name="p288515234613"></a>pthread_setcancelstate</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p8799217101512"><a name="p8799217101512"></a><a name="p8799217101512"></a>设置线程可取消状态。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p488518224620"><a name="p488518224620"></a><a name="p488518224620"></a>-</p>
</td>
</tr>
<tr id="row1288520284619"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p158851125462"><a name="p158851125462"></a><a name="p158851125462"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p188511211463"><a name="p188511211463"></a><a name="p188511211463"></a><span>pthread_create</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p18798171712153"><a name="p18798171712153"></a><a name="p18798171712153"></a>创建一个新的线程。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p14886192124615"><a name="p14886192124615"></a><a name="p14886192124615"></a>-</p>
</td>
</tr>
<tr id="row1288614204611"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p388610218462"><a name="p388610218462"></a><a name="p388610218462"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p108861274620"><a name="p108861274620"></a><a name="p108861274620"></a><span>pthread_detach</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p379831718156"><a name="p379831718156"></a><a name="p379831718156"></a>分离一个线程。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p38861126461"><a name="p38861126461"></a><a name="p38861126461"></a>-</p>
</td>
</tr>
<tr id="row188614213467"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p14886826461"><a name="p14886826461"></a><a name="p14886826461"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p108861527469"><a name="p108861527469"></a><a name="p108861527469"></a><span>pthread_equal</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p18799817181516"><a name="p18799817181516"></a><a name="p18799817181516"></a>比较两个线程ID是否相等。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p588612219467"><a name="p588612219467"></a><a name="p588612219467"></a>-</p>
</td>
</tr>
<tr id="row1488619294613"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p108867214619"><a name="p108867214619"></a><a name="p108867214619"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p988752164613"><a name="p988752164613"></a><a name="p988752164613"></a><span>pthread_exit</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p979871741512"><a name="p979871741512"></a><a name="p979871741512"></a>终止正在调用的线程。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p28871522460"><a name="p28871522460"></a><a name="p28871522460"></a>-</p>
</td>
</tr>
<tr id="row88871220467"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p188877244617"><a name="p188877244617"></a><a name="p188877244617"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p188879212461"><a name="p188879212461"></a><a name="p188879212461"></a><span>pthread_getschedparam</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p158001717101517"><a name="p158001717101517"></a><a name="p158001717101517"></a>获取线程的调度策略和参数。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p198871128465"><a name="p198871128465"></a><a name="p198871128465"></a><span id="text138623004915"><a name="text138623004915"></a><a name="text138623004915"></a>OpenHarmony</span>：支持SCHED_FIFO 、SCHED_RR调度策略。</p>
</td>
</tr>
<tr id="row198871527462"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1188715284613"><a name="p1188715284613"></a><a name="p1188715284613"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p088715274620"><a name="p088715274620"></a><a name="p088715274620"></a><span>pthread_join</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p2079921719159"><a name="p2079921719159"></a><a name="p2079921719159"></a>等待指定的线程结束。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p2088815214462"><a name="p2088815214462"></a><a name="p2088815214462"></a>-</p>
</td>
</tr>
<tr id="row13888142184617"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p688802124614"><a name="p688802124614"></a><a name="p688802124614"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1088820215469"><a name="p1088820215469"></a><a name="p1088820215469"></a>pthread_self</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1179931761515"><a name="p1179931761515"></a><a name="p1179931761515"></a>获取当前线程的ID。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p588802164611"><a name="p588802164611"></a><a name="p588802164611"></a>-</p>
</td>
</tr>
<tr id="row15888132124614"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p8888627462"><a name="p8888627462"></a><a name="p8888627462"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1888811214620"><a name="p1888811214620"></a><a name="p1888811214620"></a>pthread_setschedprio</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1888816219469"><a name="p1888816219469"></a><a name="p1888816219469"></a>设置线程的调度静态优先级。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p48881822463"><a name="p48881822463"></a><a name="p48881822463"></a>-</p>
</td>
</tr>
<tr id="row12889142194616"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1988915284613"><a name="p1988915284613"></a><a name="p1988915284613"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p148891026466"><a name="p148891026466"></a><a name="p148891026466"></a>pthread_kill</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p128001017121510"><a name="p128001017121510"></a><a name="p128001017121510"></a>向线程发送信号。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p9889522466"><a name="p9889522466"></a><a name="p9889522466"></a>-</p>
</td>
</tr>
<tr id="row19889624465"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p0889620469"><a name="p0889620469"></a><a name="p0889620469"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p58894244612"><a name="p58894244612"></a><a name="p58894244612"></a>pthread_once</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p4801617171515"><a name="p4801617171515"></a><a name="p4801617171515"></a>使函数调用只能执行一次。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p16889122204617"><a name="p16889122204617"></a><a name="p16889122204617"></a>-</p>
</td>
</tr>
<tr id="row288917219462"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1688913294617"><a name="p1688913294617"></a><a name="p1688913294617"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p2889152174613"><a name="p2889152174613"></a><a name="p2889152174613"></a>pthread_atfork</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p198899219461"><a name="p198899219461"></a><a name="p198899219461"></a>注册fork的处理程序。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p2889520467"><a name="p2889520467"></a><a name="p2889520467"></a>-</p>
</td>
</tr>
<tr id="row988922114611"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p88897234618"><a name="p88897234618"></a><a name="p88897234618"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1588952114611"><a name="p1588952114611"></a><a name="p1588952114611"></a>pthread_cleanup_pop</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p2048113619157"><a name="p2048113619157"></a><a name="p2048113619157"></a>删除位于清理处理程序堆栈顶部的例程。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p178901424460"><a name="p178901424460"></a><a name="p178901424460"></a>-</p>
</td>
</tr>
<tr id="row188906284610"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p8890152134616"><a name="p8890152134616"></a><a name="p8890152134616"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p589017244615"><a name="p589017244615"></a><a name="p589017244615"></a>pthread_cleanup_push</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p114823610159"><a name="p114823610159"></a><a name="p114823610159"></a>将例程推送到清理处理程序堆栈的顶部。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1890828463"><a name="p1890828463"></a><a name="p1890828463"></a>-</p>
</td>
</tr>
<tr id="row189012284618"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p68908224615"><a name="p68908224615"></a><a name="p68908224615"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1589011234615"><a name="p1589011234615"></a><a name="p1589011234615"></a>pthread_barrier_destroy</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p188901723467"><a name="p188901723467"></a><a name="p188901723467"></a>销毁屏障对象（高级实时线程）</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p88902244620"><a name="p88902244620"></a><a name="p88902244620"></a>-</p>
</td>
</tr>
<tr id="row089015218467"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1689011254619"><a name="p1689011254619"></a><a name="p1689011254619"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p48908214468"><a name="p48908214468"></a><a name="p48908214468"></a>pthread_barrier_init</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p7890172124617"><a name="p7890172124617"></a><a name="p7890172124617"></a>初始化屏障对象（高级实时线程）</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1689015217461"><a name="p1689015217461"></a><a name="p1689015217461"></a>-</p>
</td>
</tr>
<tr id="row8890182114615"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p118907215468"><a name="p118907215468"></a><a name="p118907215468"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p128902215466"><a name="p128902215466"></a><a name="p128902215466"></a>pthread_barrier_wait</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1189112204618"><a name="p1189112204618"></a><a name="p1189112204618"></a>屏障同步（高级实时线程）</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p208911722464"><a name="p208911722464"></a><a name="p208911722464"></a>-</p>
</td>
</tr>
<tr id="row589110216461"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p08911826466"><a name="p08911826466"></a><a name="p08911826466"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p18891321469"><a name="p18891321469"></a><a name="p18891321469"></a>pthread_barrierattr_destroy</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1048203611514"><a name="p1048203611514"></a><a name="p1048203611514"></a>销毁屏障属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1989112124612"><a name="p1989112124612"></a><a name="p1989112124612"></a>-</p>
</td>
</tr>
<tr id="row9891624468"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p108914214465"><a name="p108914214465"></a><a name="p108914214465"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1289182104618"><a name="p1289182104618"></a><a name="p1289182104618"></a>pthread_barrierattr_init</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1848113618159"><a name="p1848113618159"></a><a name="p1848113618159"></a>初始化屏障属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p12891202104615"><a name="p12891202104615"></a><a name="p12891202104615"></a>-</p>
</td>
</tr>
<tr id="row118914214464"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1289116214465"><a name="p1289116214465"></a><a name="p1289116214465"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1989116254616"><a name="p1989116254616"></a><a name="p1989116254616"></a><span>pthread_mutex_destroy</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p3891927469"><a name="p3891927469"></a><a name="p3891927469"></a>销毁互斥锁。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1189111220464"><a name="p1189111220464"></a><a name="p1189111220464"></a>-</p>
</td>
</tr>
<tr id="row18891326468"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p68915219469"><a name="p68915219469"></a><a name="p68915219469"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p689212124610"><a name="p689212124610"></a><a name="p689212124610"></a><span>pthread_mutex_init</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p189262174615"><a name="p189262174615"></a><a name="p189262174615"></a>初始化互斥锁。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p089216210461"><a name="p089216210461"></a><a name="p089216210461"></a>-</p>
</td>
</tr>
<tr id="row1689213216461"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p38923244615"><a name="p38923244615"></a><a name="p38923244615"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p989216213462"><a name="p989216213462"></a><a name="p989216213462"></a><span>pthread_mutex_lock</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1089216218469"><a name="p1089216218469"></a><a name="p1089216218469"></a>互斥锁加锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p58921028469"><a name="p58921028469"></a><a name="p58921028469"></a>-</p>
</td>
</tr>
<tr id="row989214284614"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p168927213469"><a name="p168927213469"></a><a name="p168927213469"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1389262154612"><a name="p1389262154612"></a><a name="p1389262154612"></a><span>pthread_mutex_trylock</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p9892182114619"><a name="p9892182114619"></a><a name="p9892182114619"></a>互斥锁尝试加锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p28926213469"><a name="p28926213469"></a><a name="p28926213469"></a>-</p>
</td>
</tr>
<tr id="row1989218264610"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1892122164617"><a name="p1892122164617"></a><a name="p1892122164617"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p8893132114614"><a name="p8893132114614"></a><a name="p8893132114614"></a><span>pthread_mutex_unlock</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p7893182154611"><a name="p7893182154611"></a><a name="p7893182154611"></a>互斥锁解锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1689318210466"><a name="p1689318210466"></a><a name="p1689318210466"></a>-</p>
</td>
</tr>
<tr id="row10893192194614"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1789317254618"><a name="p1789317254618"></a><a name="p1789317254618"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1089320217465"><a name="p1089320217465"></a><a name="p1089320217465"></a><span>pthread_mutexattr_destroy</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p168933274614"><a name="p168933274614"></a><a name="p168933274614"></a>销毁互斥锁属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p11893326462"><a name="p11893326462"></a><a name="p11893326462"></a>-</p>
</td>
</tr>
<tr id="row7893523465"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p128937234619"><a name="p128937234619"></a><a name="p128937234619"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p38931922469"><a name="p38931922469"></a><a name="p38931922469"></a><span>pthread_mutexattr_gettype</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p547173614155"><a name="p547173614155"></a><a name="p547173614155"></a>获取互斥锁类型属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p118932211469"><a name="p118932211469"></a><a name="p118932211469"></a>-</p>
</td>
</tr>
<tr id="row15893526464"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1989416284611"><a name="p1989416284611"></a><a name="p1989416284611"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1389413212461"><a name="p1389413212461"></a><a name="p1389413212461"></a><span>pthread_mutexattr_init</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p447133671516"><a name="p447133671516"></a><a name="p447133671516"></a>初始化互斥锁属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1189415254616"><a name="p1189415254616"></a><a name="p1189415254616"></a>-</p>
</td>
</tr>
<tr id="row1894102194616"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1289432134614"><a name="p1289432134614"></a><a name="p1289432134614"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p18941928465"><a name="p18941928465"></a><a name="p18941928465"></a><span>pthread_mutexattr_settype</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1247203611159"><a name="p1247203611159"></a><a name="p1247203611159"></a>设置互斥锁类型属性。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p168941328465"><a name="p168941328465"></a><a name="p168941328465"></a>-</p>
</td>
</tr>
<tr id="row48942215466"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p108942217463"><a name="p108942217463"></a><a name="p108942217463"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1189418216468"><a name="p1189418216468"></a><a name="p1189418216468"></a>pthread_mutex_timedlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p580191751513"><a name="p580191751513"></a><a name="p580191751513"></a>使用超时锁定互斥锁。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p0894122134613"><a name="p0894122134613"></a><a name="p0894122134613"></a>-</p>
</td>
</tr>
<tr id="row1894122134612"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p689414212466"><a name="p689414212466"></a><a name="p689414212466"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1189517234613"><a name="p1189517234613"></a><a name="p1189517234613"></a>pthread_rwlock_destroy</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p7895429466"><a name="p7895429466"></a><a name="p7895429466"></a>销毁读写锁。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p10895122174617"><a name="p10895122174617"></a><a name="p10895122174617"></a>-</p>
</td>
</tr>
<tr id="row989562144613"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p389542184611"><a name="p389542184611"></a><a name="p389542184611"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p13895142104612"><a name="p13895142104612"></a><a name="p13895142104612"></a>pthread_rwlock_init</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p0895172114615"><a name="p0895172114615"></a><a name="p0895172114615"></a>初始化读写锁。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p78951827462"><a name="p78951827462"></a><a name="p78951827462"></a>-</p>
</td>
</tr>
<tr id="row118953217461"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p689516216461"><a name="p689516216461"></a><a name="p689516216461"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p58955244611"><a name="p58955244611"></a><a name="p58955244611"></a>pthread_rwlock_rdlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p15803181719154"><a name="p15803181719154"></a><a name="p15803181719154"></a>获取读写锁读锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p889502164620"><a name="p889502164620"></a><a name="p889502164620"></a>-</p>
</td>
</tr>
<tr id="row689515218467"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p178956218463"><a name="p178956218463"></a><a name="p178956218463"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p17895152134618"><a name="p17895152134618"></a><a name="p17895152134618"></a>pthread_rwlock_timedrdlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p5803181721513"><a name="p5803181721513"></a><a name="p5803181721513"></a>使用超时锁定读写锁读锁。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1589622114618"><a name="p1589622114618"></a><a name="p1589622114618"></a>-</p>
</td>
</tr>
<tr id="row789615254618"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p168961622467"><a name="p168961622467"></a><a name="p168961622467"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1889612124610"><a name="p1889612124610"></a><a name="p1889612124610"></a>pthread_rwlock_timedwrlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p11431336191520"><a name="p11431336191520"></a><a name="p11431336191520"></a>使用超时锁定读写锁写锁。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p188966244617"><a name="p188966244617"></a><a name="p188966244617"></a>-</p>
</td>
</tr>
<tr id="row38966284617"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1389620264616"><a name="p1389620264616"></a><a name="p1389620264616"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p138961222469"><a name="p138961222469"></a><a name="p138961222469"></a>pthread_rwlock_tryrdlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1780314172156"><a name="p1780314172156"></a><a name="p1780314172156"></a>尝试获取读写锁读锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p989642174614"><a name="p989642174614"></a><a name="p989642174614"></a>-</p>
</td>
</tr>
<tr id="row20896142154616"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1689622204620"><a name="p1689622204620"></a><a name="p1689622204620"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p38981620462"><a name="p38981620462"></a><a name="p38981620462"></a>pthread_rwlock_trywrlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1443936191520"><a name="p1443936191520"></a><a name="p1443936191520"></a>尝试获取读写锁写锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p128981725468"><a name="p128981725468"></a><a name="p128981725468"></a>-</p>
</td>
</tr>
<tr id="row489815210461"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p18899821465"><a name="p18899821465"></a><a name="p18899821465"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1789913217469"><a name="p1789913217469"></a><a name="p1789913217469"></a>pthread_rwlock_unlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p143193651512"><a name="p143193651512"></a><a name="p143193651512"></a>读写锁解锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p390010211465"><a name="p390010211465"></a><a name="p390010211465"></a>-</p>
</td>
</tr>
<tr id="row1890032124612"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p159009219466"><a name="p159009219466"></a><a name="p159009219466"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1190010214469"><a name="p1190010214469"></a><a name="p1190010214469"></a>pthread_rwlock_wrlock</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p14803517111519"><a name="p14803517111519"></a><a name="p14803517111519"></a>获取读写锁写锁操作。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1390011294618"><a name="p1390011294618"></a><a name="p1390011294618"></a>-</p>
</td>
</tr>
<tr id="row590032124613"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p890022144619"><a name="p890022144619"></a><a name="p890022144619"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1290010244614"><a name="p1290010244614"></a><a name="p1290010244614"></a>pthread_rwlockattr_destroy</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p154719365157"><a name="p154719365157"></a><a name="p154719365157"></a>销毁读写锁属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p19900428461"><a name="p19900428461"></a><a name="p19900428461"></a>-</p>
</td>
</tr>
<tr id="row190042174617"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1190010284610"><a name="p1190010284610"></a><a name="p1190010284610"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1890017217462"><a name="p1890017217462"></a><a name="p1890017217462"></a>pthread_rwlockattr_init</p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p247133661511"><a name="p247133661511"></a><a name="p247133661511"></a>初始化读写锁属性对象。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p17900192154611"><a name="p17900192154611"></a><a name="p17900192154611"></a>-</p>
</td>
</tr>
<tr id="row10900320461"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p5901229462"><a name="p5901229462"></a><a name="p5901229462"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p149018294618"><a name="p149018294618"></a><a name="p149018294618"></a><span>pthread_cond_broadcast</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p16802181771514"><a name="p16802181771514"></a><a name="p16802181771514"></a>解除若干已被等待条件阻塞的线程。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1290118264619"><a name="p1290118264619"></a><a name="p1290118264619"></a>-</p>
</td>
</tr>
<tr id="row590115234620"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p15901202194613"><a name="p15901202194613"></a><a name="p15901202194613"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1190119212466"><a name="p1190119212466"></a><a name="p1190119212466"></a><span>pthread_cond_destroy</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p19802017191512"><a name="p19802017191512"></a><a name="p19802017191512"></a>销毁条件变量。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p1390122114610"><a name="p1390122114610"></a><a name="p1390122114610"></a>-</p>
</td>
</tr>
<tr id="row1890192164611"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p3901112204616"><a name="p3901112204616"></a><a name="p3901112204616"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1890102184618"><a name="p1890102184618"></a><a name="p1890102184618"></a><span>pthread_cond_init</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p48025173153"><a name="p48025173153"></a><a name="p48025173153"></a>初始化条件变量。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p119011210466"><a name="p119011210466"></a><a name="p119011210466"></a>-</p>
</td>
</tr>
<tr id="row129011214615"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p9902523468"><a name="p9902523468"></a><a name="p9902523468"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p59021722460"><a name="p59021722460"></a><a name="p59021722460"></a><span>pthread_cond_signal</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p58024177158"><a name="p58024177158"></a><a name="p58024177158"></a>解除被阻塞的线程。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p179022244615"><a name="p179022244615"></a><a name="p179022244615"></a>-</p>
</td>
</tr>
<tr id="row13902423461"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p69022025464"><a name="p69022025464"></a><a name="p69022025464"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1190252134620"><a name="p1190252134620"></a><a name="p1190252134620"></a><span>pthread_cond_timedwait</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p158024175151"><a name="p158024175151"></a><a name="p158024175151"></a>定时等待条件。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p109020264613"><a name="p109020264613"></a><a name="p109020264613"></a>-</p>
</td>
</tr>
<tr id="row189022218464"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p09021228463"><a name="p09021228463"></a><a name="p09021228463"></a>pthread.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p3902324460"><a name="p3902324460"></a><a name="p3902324460"></a><span>pthread_cond_wait</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p1690262154612"><a name="p1690262154612"></a><a name="p1690262154612"></a>等待条件。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p19902122104611"><a name="p19902122104611"></a><a name="p19902122104611"></a>-</p>
</td>
</tr>
<tr id="row159027218467"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p990318212464"><a name="p990318212464"></a><a name="p990318212464"></a>semaphore.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p109039234617"><a name="p109039234617"></a><a name="p109039234617"></a><span>sem_destroy</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p195081328171710"><a name="p195081328171710"></a><a name="p195081328171710"></a>销毁指定的无名信号量。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p190318214460"><a name="p190318214460"></a><a name="p190318214460"></a>-</p>
</td>
</tr>
<tr id="row1690342194611"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1490318217469"><a name="p1490318217469"></a><a name="p1490318217469"></a>semaphore.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p0903162124610"><a name="p0903162124610"></a><a name="p0903162124610"></a><span>sem_getvalue</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p650892891712"><a name="p650892891712"></a><a name="p650892891712"></a>获得指定信号量计数值。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p69036234614"><a name="p69036234614"></a><a name="p69036234614"></a>-</p>
</td>
</tr>
<tr id="row1490312214464"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p159031284618"><a name="p159031284618"></a><a name="p159031284618"></a>semaphore.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1790315254617"><a name="p1790315254617"></a><a name="p1790315254617"></a><span>sem_init</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p2508328121711"><a name="p2508328121711"></a><a name="p2508328121711"></a>创建并初始化一个无名信号量。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p129038211463"><a name="p129038211463"></a><a name="p129038211463"></a>-</p>
</td>
</tr>
<tr id="row1490416211462"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p3904126469"><a name="p3904126469"></a><a name="p3904126469"></a>semaphore.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p090416220464"><a name="p090416220464"></a><a name="p090416220464"></a><span>sem_post</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p6508162813176"><a name="p6508162813176"></a><a name="p6508162813176"></a>增加信号量计数。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p090414218463"><a name="p090414218463"></a><a name="p090414218463"></a>-</p>
</td>
</tr>
<tr id="row14904162164618"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p1590432194620"><a name="p1590432194620"></a><a name="p1590432194620"></a>semaphore.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p29041822467"><a name="p29041822467"></a><a name="p29041822467"></a><span>sem_timedwait</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p139049213468"><a name="p139049213468"></a><a name="p139049213468"></a>获取信号量，且有超时返回功能。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p690452114613"><a name="p690452114613"></a><a name="p690452114613"></a>-</p>
</td>
</tr>
<tr id="row390411211468"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p199049214612"><a name="p199049214612"></a><a name="p199049214612"></a>semaphore.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p1190413217467"><a name="p1190413217467"></a><a name="p1190413217467"></a><span>sem_trywait</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p18509102891714"><a name="p18509102891714"></a><a name="p18509102891714"></a>尝试获取信号量。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p39048213469"><a name="p39048213469"></a><a name="p39048213469"></a>-</p>
</td>
</tr>
<tr id="row3905152174616"><td class="cellrowborder" valign="top" width="14.299999999999999%" headers="mcps1.2.5.1.1 "><p id="p690513215466"><a name="p690513215466"></a><a name="p690513215466"></a>semaphore.h</p>
</td>
<td class="cellrowborder" valign="top" width="28.599999999999998%" headers="mcps1.2.5.1.2 "><p id="p15905926462"><a name="p15905926462"></a><a name="p15905926462"></a><span>sem_wait</span></p>
</td>
<td class="cellrowborder" valign="top" width="22.38%" headers="mcps1.2.5.1.3 "><p id="p950912813172"><a name="p950912813172"></a><a name="p950912813172"></a>获取信号量。</p>
</td>
<td class="cellrowborder" valign="top" width="34.72%" headers="mcps1.2.5.1.4 "><p id="p109051725466"><a name="p109051725466"></a><a name="p109051725466"></a>-</p>
</td>
</tr>
</tbody>
</table>
