# 监控系统

## 监控类型

+ 应用性能监控 (Application Performance Monitoring)
+ 业务交易监控 (Business Transaction Monitoring)
+ 网络性能监控 (Network Monitoring)
+ 操作系统监控 (System Monitoring)
+ 网络站点监控 (Website Monitoring)

## 实现原理

### 模块组成

+ 数据采集部分 (客户端, Agent)
+ 数据存储分析告警展示部分 (服务器端, Server)

### 采集协议

+ 私有协议
  + 专用客户端
+ 公用协议
  + SNMP
  + IPMI
  + SSH
  + Telnet

### 采集模式

+ 被动模式, pull, 从服务器端到客户端采集数据
+ 主动模式, push, 客户端主动上报数据到服务器端

### 监控指标

常见监控指标：
+ 主机
  + cpu, 内存, 磁盘的剩余空间/利用率和I/O
  + SWAP利用率
  + 系统启动时间、进程数、负载
+ 网卡
  + ping的响应时间以及数据包发送的成功率
  + 网卡流入/流出量和错误的数据包数
+ 文件
  + 文件大小、指纹哈希值、匹配查询、字符串存在与否
+ URL
  + 指定URL访问过程中的返回码
  + 下载时间及文件大小
  + 返回数据的匹配
+ 应用程序
  + 端口、服务状态
  + 内存、cpu使用率
  + 请求数、并发连接数
  + 消息队列的字节数
  + 客户端事务处理数
+ 数据库
  + 指定表空间
  + 游标数、当前连接数
  + 会话数、事务数、死锁数
  + 缓冲池命中率、库缓存命中率
  + 进程的内存利用率
+ 日志
  + 错误日志匹配
  + 特定字符串匹配
+ 硬件
  + 温度、风扇转速
  + 电压、电源
  + 主板控制器
  + 磁盘阵列

## 数据存储

+ 本地存储, 使用本地磁盘, 基于文件的方式存储
+ 时序数据库, 如OpenTSDB、Graphite、InfluxDB、Prometheus等
+ 数据库管理系统, 如MySql、Oracle、Sql Server等
+ NoSql数据库
+ 列存储数据库, 如HBase
+ 全文搜索引擎数据库, 如Elasticsearch

## 告警功能

根据设定的阈值进行告警，同时也要求在发生故障时有一定的故障自动化处理功能，对于特殊的告警还需要具备告警的升级功能，将不同级别的告警分成不同的梯度发送给不同的告警接收人。

## 开源产品

+ 流量监控
  + MRTG
  + Cacti
  + SmokePing
+ 性能告警
  + Nagios
  + Zabbix
  + Zenoss Core
  + Ganglia
  + Netdata
+ 基于时序数据库
  + Graphite
  + OpenTSDB
  + InfluxDB
  + Prometheus
  + OpenFalcon
+ 基于全文搜索引擎数据库
  + ELK

