[TOC]

## 一、分布式系统中的相关概念

### 1.1、大型互联网架构目标

#### 1.1.1、传统项目与互联网项目

传统项目和互联网项目的核心差异在于**用户群体**。

传统项目（i.e. OA、HR、CRM）主要面向企业员工，互联网项目（i.e. 微信、百度、淘宝）面向网民。

- 显而易见两者在用户数量上存在巨大差异，在某些处理（i.e. 并发处理）上有所不同。
- 两者之间对于项目的忍耐力也有所不同，对于互联网项目的用户，其忍耐力为零，看重用户体验（美观、功能、速度、稳定性）。



互联网项目的特点：

- 用户数量庞大
- 流量大，并发高
- 海量数据
- 易受攻击
- 功能繁琐
- 变更快
- ...



#### 1.1.2、互联网架构目标

* 高性能：提供快速的访问体验

>衡量网站的性能指标：
>
>- 响应时间：执行一个请求从开始到最后受到响应数据所花费的时间。
>- 并发数：系统同时能够处理的请求数量。
>  - 并发连接数：客户端向服务器发起请求，并建立起TCP连接，每秒钟服务器连接的总TCP数量。
>  - 请求数：QPS（Query Per Second）每秒多少请求。
>  - 并发用户数：单位时间内有多少个用户。
>- 吞吐量：单位时间内系统能够处理的请求数量。
>  - QPS：Query Per Second 每秒查询数
>  - TPS：Transaction Per Second 每秒事务数
>  - 一个事务是指一个客户机向服务器发送请求，服务器做出反应的过程。客户机在发送请求时开始计时，受到服务器响应后结束计时，以此计算使用的时间和完成的事务个数。



* 高可用：网站服务一直可以正常访问

- 可伸缩：通过硬件增加/减少，提高/降低处理能力
- 高可扩展：系统内耦合低，方便通过增加/移除方式，增加/减少新的模块/功能
- 安全性：提供网站安全访问和数据加密，安全存储等策略
- 敏捷性：快速响应需求变更，以便抢占市场



### 1.2、集群和分布式

* 集群：很多机器，干同一件事。
* 分布式：很多机器，干不一样事，这些不一样的事可以组合成一件大事。