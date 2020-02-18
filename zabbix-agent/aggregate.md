# 聚合型监控方式

聚合型（Aggregate）监控方式是指将已经存储在数据库中的监控指标数值进行二次统计运算，从而形成新的监控指标。

```
groupfunc["Host group", "Item key", itemfunc,timeperiod]
grpmax    主机分组        key名称   max       30s
grpmin                              min       5m
grpavg                              avg       1h
grpsum                              sum       1d
                                    count     1w
                                    last

当itemfunc为last时，后面不能接时间参数timeperiod，这是由于使用
last时，就会取Item key的第一个值
```

+ 聚合型监控方式源码分析, src/zabbix_server/poller/checks_aggregate.c

```
# 对MySQL Servers组监控项最近一次监控数据求和
grpsum["MySQL Servers","vfs.fs.size[/,total]",last]

grpavg["MySQL Servers","system.cpu.load[,avg1]",last]

# 对MySQL Servers组监控项mysql.qps，最近5分钟内每个主机所获取
# 到的监控数据的平均值，再求平均值
grpavg["MySQL Servers",mysql.qps,avg,5m]

grpavg[["Servers A","Servers B","Servers C"],system.cpu.load,last]

grpsum[["WEB-1","WEB-2","WEB-3"],nginx.404.log ,sum,10m]

grpsum["Linux",log[/var/log/message,error],count,30m]
```
